# Jars — a 6-jar money tracker

A small, private web app for managing money with the **6-jar system** (the JARS method
from T. Harv Eker's *Secrets of the Millionaire Mind*). Income is split across six jars by
percentage; you log expenses against a jar and watch what's left — for the current month,
or across all time. It also tracks a running account balance and what you owe on each
credit card.

**Live:** https://jars-tracker.vercel.app — deployed on Vercel from this repo, so every
commit to the main branch redeploys the site automatically.

## The six jars

| Jar | Default | What it's for |
| --- | --- | --- |
| Necessities | 50% | Rent, food, bills, transport |
| Financial Freedom | 10% | Investing only — never spent |
| Long-Term Savings | 10% | Big future purchases, reserves |
| Education | 10% | Courses, books, skills |
| Play | 10% | Guilt-free fun money |
| Give | 10% | Charity and causes |

The percentages are a starting point, not a rule — every one is editable.

## How income funds your months

Income you earn in a month funds the **following** month's jars — the "live on last
month's income" buffer, for better cashflow. So June's earnings fund July's jars.

What matters is when income was **earned**, not when it landed. Each income entry has a
Received date plus an optional **earned period** (from / to). A paycheck for late-July work
that arrives in early August still funds August, because it was earned in July. If a pay
period straddles two months, the amount is split by days — a Jul 22–Aug 4 paycheck puts its
July days toward August and its August days toward September. Leave the earned period blank
and it falls back to the received date.

On the dashboard, the **Income to allocate** figure for a month is exactly this: last
month's earnings, the money available to spread across the jars now.

## Two views: this month and all time

A month bar lets you move between months or switch to **All time**. It opens on the current
month. Switching scope is only a lens — it never changes your data.

In the **month view**, every jar resets each month:

- **Spending jars** (Necessities, Education, Play) show what's left to spend this month.
- **Accumulating jars** (Financial Freedom, Long-Term Savings, Give) show progress toward
  that month's savings goal (the month's allocation), with a green **goal met** badge when
  you don't dip in.

In **All time**, jars show the cumulative picture (lifetime remaining, or accumulated
balance against the lifetime goal).

## The dashboard totals

A strip of stats sits above the tabs, all following the selected scope:

- **Income to allocate** — last month's earnings feeding this month (or total income, in All time).
- **Total spending** and **Total accumulating** — spent across spending jars, and net-saved
  into accumulating jars, for the scope.
- **Account balance** — your received income minus all expenses, carried forward through the
  end of the viewed month (see the caveat below). Turns red if negative.
- **Credit card total** — sum of card spending; tap it to expand a per-card breakdown.

## Why it's built the way it is

**The two jar types behave differently.** Spending jars are meant to be spent down (Play
especially — a full Play jar is a nudge to enjoy it). Accumulating jars are meant to grow;
the month view frames them as monthly savings goals, while All time keeps their cumulative
balance.

**One purchase, one entry — and never log the credit-card bill.** A jar tracks a *budget*,
not the cash in your bank. Every real purchase decrements exactly one jar, once, whether
paid by card or cash — a card is a payment method, not a category. The monthly card bill is
a debt settlement, not an expense; those purchases were already counted. A nice payoff:
because you log purchases and never the bill, **each card's total for a month is your
expected statement for that card.**

**Account balance is a net position, not live bank cash.** Because a card purchase is logged
the day you buy but the bill payment never is, this number drops the moment you charge
something — even though the cash leaves your bank weeks later. So read it as "what I'd have
left if everything I've spent were settled," not the amount in your account today. It uses
your *received* income (real cash in), separate from the "Income to allocate" budget number.

**Refunds are negative expenses.** Logging a refund stores a negative amount against a jar,
so it automatically raises what's left in that jar, lowers total spending, lifts the account
balance, and — if it hit a card — reduces that card's total. No special-casing needed.

**Balances are recomputed from the log.** Every figure is derived fresh from the dated income
and expense lists rather than stored running totals. That's what let the monthly view, the
funding model, and all the totals ship without any data migration.

**The percentages are yours to change**, with a checker that they total 100%, and you can add
or remove jars.

## What's been built

**1. A spreadsheet (`six_jar_tracker.xlsx`) — the "brain."** Built first to settle the math
and data model: configurable jars with a 100% checker, income and expense logs, and a
dashboard that allocates income and shows allocated / spent / remaining per jar.

**2. The web app (`index.html`) — the daily tool.** A single self-contained file: all styling
and logic inside it, nothing loaded from the internet, data stored locally in your browser.

- Three tabs (Jars, Income, Settings) plus an always-on **Add expense** button.
- Each jar is a vessel that fills with its colour; overspent jars turn red.
- Month-by-month and All-time views; the funding model with optional earned periods.
- Add, **edit**, and delete both expenses and income (a pencil icon reopens an entry filled).
- An **Expense / Refund** toggle; refunds are stored negative and shown as green +amounts.
- Payment method per expense (Cash/Debit or a named credit card); free-text card names group
  case-insensitively.
- Dashboard totals: income to allocate, spending, accumulating, account balance, and a
  credit-card total with an expandable per-card breakdown.
- Recent activity (latest 8, with **Show all**), two-decimal amounts throughout.
- **Export / Import** backups as JSON to move data between browsers — export uses the phone
  share sheet (Save to Files / AirDrop) and a download on desktop.
- Tuned for phone and desktop.

## Using the app

1. Open https://jars-tracker.vercel.app (or open `index.html` directly in a browser).
2. **Income** tab: add your take-home pay with its Received date, and optionally the earned
   period so it funds the right month.
3. **Add expense**: choose Expense or Refund, enter the amount, pick a jar and the payment
   method (name the card if you paid by card), save.
4. **Jars** tab: the month bar moves between months or shows All time; the totals strip and
   card breakdown sit above the tabs. **Settings** tunes percentages, renames jars, and holds
   Export / Import / Erase.

On a phone, open the live URL and "Add to Home Screen" for a full-screen app. Updating is
just a commit: upload a new `index.html` to this repo and Vercel republishes within a minute.

## Known limitations (deliberate, for now)

- **Account balance is a net position, not live bank cash** — because card purchases count
  before the bill clears and bill payments aren't logged.
- **Data lives in one browser** — your phone and laptop keep separate, unsynced data.
  Export/Import moves it between them.
- **Reset only, no carry-over** — unspent money in a spending jar doesn't roll into next
  month in the month view (All time still holds the real lifetime totals).
- **Percentages are global** — a single live setting, so changing one recomputes past months.
- **The first earned month is unfunded** — its income funds the next month, so you need one
  buffer month to start.

## Roadmap

- **Sign-in so data syncs across mobile and desktop** — an account with server-side storage.
  The biggest step: needs a real backend (server, database, auth) and changes the privacy
  model, so it will be opt-in.
- **Per-jar rollover** — an option to carry unspent money forward month to month.
- **Frozen history** — snapshot each income's allocation at entry time so changing
  percentages no longer rewrites past months.

## Privacy

Today there is no server and no account: your income and expenses never leave the browser on
your device. The sign-in feature on the roadmap would trade some of that privacy for
cross-device convenience, so it will be opt-in rather than the default.
