# Jars — a 6-jar money tracker

A small, private web app for managing money with the **6-jar system** (the JARS method
from T. Harv Eker's *Secrets of the Millionaire Mind*). Every time you get paid, your
net income is split across six jars by percentage. You log expenses against a jar and
watch what's left. No accounts, no server — everything stays in your browser.

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

## Why it's built the way it is

These are the decisions behind the app, kept here so the "why" doesn't get lost.

**The jars don't all behave the same way.** Treating all six as "budget minus spending
equals remaining" would lose the point of the system. They split into two kinds:

- *Spending jars* (Necessities, Education, Play) get funded each period and spent down.
  "Remaining" is the number that matters. Play is meant to be **emptied** — that's the
  whole idea, so a growing Play balance is a nudge to go enjoy it, not to hoard it.
- *Accumulating jars* (Financial Freedom, Long-Term Savings, Give) grow over time and
  get drawn down only in occasional lumps (an investment, a donation, a big purchase).
  Here a rising balance is the goal. Financial Freedom is special: you never spend the
  principal, so a big number there is the win.

The app tags each jar as Spending or Accumulating and fills the jar accordingly.

**One purchase, one entry — and never log the credit-card bill.** A jar tracks a *budget*,
not the cash in your bank. So every real purchase decrements exactly one jar, once,
whether you paid by card or cash — a credit card is a payment method, not a category.
The trap is the monthly card bill: those purchases were already counted when they
happened, so logging the bill payment too would double-count everything. The bill is a
debt settlement, not an expense, and it never gets entered.

**The percentages are yours to change.** 50% for Necessities is unrealistic in many
high-cost areas, and Eker himself says to adjust. The app lets you edit every percentage
(with a checker that confirms they total 100%) and add or remove jars.

**Balances are always recomputed from the log.** Rather than storing a running total per
jar that could drift out of sync, the app derives each balance fresh from the income and
expense lists every time. The same principle is baked into the spreadsheet version.

## What's been built

**1. A spreadsheet (`six_jar_tracker.xlsx`) — the "brain."** Built first to nail down the
math and the data model before any code: configurable jars with a 100% checker, an income
log, an expense log with a jar dropdown, and a dashboard that allocates income and shows
allocated / spent / remaining per jar. It's the same model the app uses, and it doubles as
a place to validate the numbers.

**2. The web app (`index.html`) — the daily tool.** A single self-contained file: all the
styling and logic live inside it, nothing loads from the internet, and your data is stored
locally in your browser. Three tabs (Jars, Income, Settings) plus an always-on **Add
expense** button. The signature touch is the dashboard: each jar is a vessel that fills
with its color — spending jars drain as you spend, accumulating jars stay full as they
grow, and anything overspent turns red.

## Using the app

1. Open `index.html` in your browser (double-click it, or visit its hosted URL).
2. On the **Income** tab, add your take-home pay.
3. Tap **Add expense** whenever you spend — enter the amount, pick a jar, save.
4. The **Jars** tab shows what's left everywhere. **Settings** lets you tune percentages,
   rename jars, export a backup, or erase your data.

To use it on a phone, open the file in your mobile browser and "Add to Home Screen" — it
launches full-screen like an app. To put it online, drop the file on netlify.com/drop or
enable GitHub Pages on this repo.

## Known limitations (deliberate, for now)

- **Cumulative, not monthly.** Balances are lifetime totals, so periods don't reset and
  Play won't auto-empty each month. Treat a full Play jar as your cue to spend it.
- **Jar budgets only.** The app tracks budget allocations, not your real bank balance —
  which is exactly why the "never log the card bill" rule matters.
- **Saving needs a real browser.** Local storage works when you open the file yourself; it
  won't persist inside a sandboxed preview.

## Roadmap

- Monthly periods (fresh start each month, with history to look back on)
- An Import button to pair with Export, so backups can be restored
- A live URL via GitHub Pages

## Privacy

There is no server and no account. Your income and expenses never leave your device.
