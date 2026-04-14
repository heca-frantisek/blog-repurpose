# Dev.to — release-highlights-18-0-0

## TITLE
Killing flaky time tests: Symfony Clock across a big codebase

## TAGS
symfony, php, testing, ecommerce

## COVER BLURB
How we replaced scattered `new DateTimeImmutable()` calls with Symfony Clock across the Shopsys platform in 18.0 — and why it made our test suite boring again.

## ARTICLE BODY
Every large Symfony codebase we've worked on has the same silent problem: `new \DateTimeImmutable()` sprinkled across services, entities, and validators. It works. Until it doesn't — usually around midnight UTC, on a CI runner in a different timezone, on the day before a sprint review.

In Shopsys 18.0 we adopted Symfony's `Clock` component as the codebase-wide source of "now." The change sounds trivial. It wasn't. Here's what we did, why, and what to watch for if you're considering the same.

### Why `new DateTimeImmutable()` is a trap in a platform

The appeal is obvious: it's built into PHP, no dependency, one line. The cost only shows up later:

- **Tests become flaky around boundaries.** A promotion valid "until end of day" passes at 14:00 and fails at 23:59:59.9, because the assertion and the code call `new DateTimeImmutable()` microseconds apart.
- **You can't time-travel.** Want to assert that a scheduled export runs correctly at 03:00? You either mock the entire system clock (fragile) or you restructure the test (expensive).
- **Date arithmetic leaks into domain code.** "Now plus 14 days" for an EU order withdrawal deadline ends up computed in five different services, each with its own rounding.

We had all three. The withdrawal-from-contract feature (#4246, also in this release) was the one that finally pushed us — we were not going to ship a legally-binding 14-day deadline on top of `new \DateTimeImmutable()`.

### The before/after

Here's a representative slice of what the shift looks like in service code.

```php
// Before — "now" is whatever the OS says at the exact moment this line runs.
final class OrderWithdrawalFacade
{
    public function isWithinWithdrawalPeriod(Order $order, int $deadlineInDays): bool
    {
        $deliveredAt = $order->getDeliveredAt();
        $deadline = $deliveredAt->modify("+{$deadlineInDays} days");

        return new \DateTimeImmutable() <= $deadline;
    }
}
```

```php
// After — "now" is injected and can be frozen in tests.
use Psr\Clock\ClockInterface;

final class OrderWithdrawalFacade
{
    public function __construct(
        private readonly ClockInterface $clock,
    ) {
    }

    public function isWithinWithdrawalPeriod(Order $order, int $deadlineInDays): bool
    {
        $deliveredAt = $order->getDeliveredAt();
        $deadline = $deliveredAt->modify("+{$deadlineInDays} days");

        return $this->clock->now() <= $deadline;
    }
}
```

Wiring is boring on purpose — Symfony autowires `ClockInterface` to the native clock in prod, and we swap it in tests:

```yaml
# config/services_test.yaml
services:
    Symfony\Component\Clock\MockClock:
        arguments:
            $now: '2025-12-30 10:00:00'

    Psr\Clock\ClockInterface:
        alias: Symfony\Component\Clock\MockClock
```

A functional test that used to be flaky now reads like a spec:

```php
public function testOrderOutsideWithdrawalWindowIsRejected(): void
{
    $clock = self::getContainer()->get(MockClock::class);
    $clock->modify('2026-01-20 10:00:00'); // 21 days after delivery

    $order = $this->createDeliveredOrder(deliveredAt: '2025-12-30 10:00:00');

    self::assertFalse(
        $this->orderWithdrawalFacade->isWithinWithdrawalPeriod($order, 14),
    );
}
```

### The unglamorous parts

A few things we underestimated:

- **Doctrine entities.** You can't inject `ClockInterface` into an entity constructor. We kept `createdAt`/`updatedAt` lifecycle callbacks on `DateTimeImmutable`, but moved every domain-meaningful "now" — deadlines, expirations, validity windows — into services.
- **Static helpers.** A few legacy `DateTimeHelper::now()` statics were the worst offenders. We kept the helper signature temporarily but delegated internally to the clock, then deprecated it. Big-bang refactors are how you lose a weekend.
- **Fixtures and demo data.** A lot of fixture code was secretly calling `new DateTimeImmutable()` to seed "recent" orders. Those now read from the clock too, so a frozen-clock test gets a deterministic fixture set.
- **Third-party libraries.** Some still call `new DateTimeImmutable()` internally. That's fine — we just don't pretend our clock controls them, and we don't assert on their timestamps.

### Why it's worth the churn

The payoff isn't one big win. It's the absence of a whole class of problems:

- Test failures that only show up at specific times of day — gone.
- "It works on my machine, it fails in CI" because CI is in UTC — gone.
- Tests for scheduled logic that previously required `sleep()` or freezegun-style hacks — now one line: `$clock->modify(...)`.

Combined with the new Grid DataSource factories (#4135) that apply locale-aware collation automatically, and the async order email preparation (#4266) that took template rendering out of the checkout critical path, 18.0 is mostly a cleanup release. The kind where the diff is boring and the operational burden drops a notch.

### Takeaways
- Treat "now" as an injected dependency, not a keyword.
- `Psr\Clock\ClockInterface` + `Symfony\Component\Clock\MockClock` gives you deterministic tests with essentially zero runtime cost.
- Don't try to convert entities — keep the clock in services where the domain decisions live.
- Deprecate static time helpers gradually; don't big-bang it.

Full release → [github](https://github.com/shopsys/shopsys/releases/tag/v18.0.0?utm_source=devto&utm_medium=organic&utm_campaign=release-highlights-18-0-0)
