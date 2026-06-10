# Jars — a 6-jar money tracker

A small, private web app for managing money with the **6-jar system** (the JARS method
from T. Harv Eker's *Secrets of the Millionaire Mind*). Each time you get paid, your net
income is split across six jars by percentage. You log expenses against a jar and watch
what's left — for the current month, or across all time.

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

The percentages are a starting point, not a rule (see below).

## Two views: this month and all time

A month bar at the top of the dashboard lets you move between months or switch to an
**All time** view. It opens on the current month, since the method runs on a monthly
rhythm. Switching scope is purely a lens — it never changes your data.

In the **month view**, every jar resets each month, so each one answers a clean question:

- **Spending jars (Necessities, Education, Play):** "How much is left to spend this
  month?" The month's allocation (that month's income × the jar's %) minus what you spent.
- **Accumulating jars (Financial Freedom, Long-Term Savings, Give):** "Did I hit my
  savings target this month?" The goal is that month's allocation; the jar shows how much
  of it you kept, with a green **goal met** badge when you don't dip in.

In the **all time** view, the jars show the cumulative picture: spending jars show
lifetime budget remaining, and accumulating jars show their accumulated balance against
the lifetime goal (total income × the jar's %), with total withdrawn. The recent-activity
list is hidden in all time so you see just the totals.

## Why it's built the way it is

These are the decisions behind the app, kept here so the "why" doesn't get lost.

**The two jar types behave differently.** Treating all six as "budget minus spending"
would lose the point of the system. Spending jars are meant to be spent down (Play
especially — a full Play jar is a nudge to go enjoy it). Accumulating jars are meant to
grow; the month view reframes them as monthly savings goals, while the all-time view keeps
their cumulative balance front and centre.

**One purchase, one entry — and never log the credit-card bill.** A jar tracks a *budget*,
not the cash in your bank. Every real purchase decrements exactly one jar, once, whether
paid by card or cash — a credit card is a payment method, not a category. The monthly card
bill is a debt settlement, not an expense; those purchases were already counted when they
happened, so logging the bill would double-count everything.

**The percentages are yours to change.** 50% for Necessities is unrealistic in many
high-cost areas, and Eker himself says to adjust. The app lets you edit every percentage
(with a checker that confirms they total 100%) and add or remove jars.

**Balances are always recomputed from the log.** Rather than storing running totals that
could drift, the app derives every figure fresh from the dated income and expense lists.
That principle is what let the month view ship as a pure computed layer — no change to
stored data, and the all-time view matches the original numbers exactly.

## What's been built

**1. A spreadsheet (`six_jar_tracker.xlsx`) — the "brain."** Built first to nail down the
math and data model before any code: configurable jars with a 100% checker, an income log,
an expense log with a jar dropdown, and a dashboard that allocates income and shows
allocated / spent / remaining per jar.

**2. The web app (`index.html`) — the daily tool.** A single self-contained file: all the
styling and logic live inside it, nothing loads from the internet, and your data is stored
locally in your browser. Highlights:

- Three tabs (Jars, Income, Settings) plus an always-on **Add expense** button.
- The signature dashboard: each jar is a vessel that fills with its colour — spending jars
  drain as you spend, accumulating jars fill toward their goal, overspent jars turn red.
- Month-by-month and all-time views with the savings-goal behaviour above.
- Add, **edit**, and delete both expenses and income (a pencil icon reopens an entry
  pre-filled). Credit card is the default payment method; amounts show to two decimals.
- Recent activity shows the latest 8, with **Show all** to expand.
- **Export / Import** backups as a JSON file, to move data between browsers or devices.
  Export uses the share sheet on phones (Save to Files / AirDrop) and a download on desktop.
- Tuned for both phone and desktop, including a two-row jar editor and a mobile date picker.

## Using the app

1. Open https://finance-jars.vercel.app (or open `index.html` directly in a browser).
2. On the **Income** tab, add your take-home pay. Income funds the month it's dated in, so
   log a month's income to fund that month's jars.
3. Tap **Add expense** whenever you spend — amount, jar, save.
4. The **Jars** tab shows what's left (or saved) per jar; use the month bar to move between
   months or see all time. **Settings** tunes percentages, renames jars, and holds
   Export / Import / Erase.

On a phone, open the live URL and "Add to Home Screen" to launch it full-screen like an
app. Updates are just a commit: upload a new `index.html` to this repo and Vercel
republishes within a minute.

## Known limitations (deliberate, for now)

- **Jar budgets only.** The app tracks budget allocations, not your real bank balance —
  which is why the "never log the card bill" rule matters.
- **Data lives in one browser.** Everything is stored in the browser you use it in, so your
  phone and laptop keep separate data and don't auto-sync. Export/Import moves it between them.
- **Reset only, no carry-over.** Unspent money in a spending jar doesn't roll into next
  month in the month view (the all-time view still holds the real lifetime totals).
- **Percentages are global.** They're a single live setting, not saved per paycheck, so
  changing a percentage recomputes past months too.

## Roadmap

- **Sign-in so data syncs across mobile and desktop.** An account that stores data on a
  server so it follows you between devices. The biggest step: it needs a real backend
  (server, database, auth) and changes the privacy model below, so it will be opt-in.
- **Per-jar rollover.** An option to carry unspent money forward month to month, per jar.
- **Frozen history.** Snapshot each income's allocation at entry time so changing
  percentages no longer rewrites past months.

## Privacy

Today there is no server and no account: your income and expenses never leave the browser
on your device. The sign-in feature on the roadmap would trade some of that privacy for
cross-device convenience, so it will be opt-in rather than the default.
