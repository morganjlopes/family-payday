# Family Payday

A three step tool for the monthly points payout. Built for a kid on the iPad.

**Live at https://morganjlopes.github.io/family-payday/**

It is also a plain static file, so opening `index.html` by double clicking still works with no server.

## Putting the icon on the iPad

1. Open https://morganjlopes.github.io/family-payday/ in **Safari** on the iPad. This does not work from Chrome.
2. Tap the **Share** button, the square with the arrow pointing up.
3. Scroll down and tap **Add to Home Screen**, then tap **Add**.

It lands on the home screen as "Payday" with the coin icon and opens full screen with no Safari toolbars. The status bar and home indicator are handled with `env(safe-area-inset-*)` so nothing gets covered.

Each girl's balance is saved on the device it was entered on, so use the same iPad each month or just retype the total.

The site carries a `noindex` tag and a `robots.txt` that disallows everything, so search engines will not list it. Anyone with the link can still open it.

## The rules

All of them live in one object at the top of the script, so changing the system is a one line edit:

```js
const RULES = {
  pointsPerDollar: 10,   // 10 points = $1
  blockSize: 100,        // but points only convert in full blocks of 100
  bonusAmount: 1,        // each full block also earns a $1 saving bonus
  bonusRepeats: true     // and it pays again every payout
};
```

**Points only turn into cash 100 at a time.** Anything left over stays on the board. So every full 100 points is worth $11.00, which is $10.00 of points plus the $1.00 saving bonus.

1028 points is 10 full hundreds, worth $110.00, with 28 points staying on the board.

The bonus is paid on the standing balance at every payout, so 250 saved points earns $2 this month, $2 next month, and so on.

## The three options

At 1028 points:

| Option | Cash today | Points after |
|---|---|---|
| Cash Out | $110.00 | 28 |
| Bonus Only | $10.00 | 1028 |
| Roll Over | $0.00 | 1128 |

Cash Out converts every full hundred and leaves the remainder on the board. Bonus Only pays the bonus and keeps every point. Roll Over converts the bonus to points at 10 points per $1 and pays no cash.

Below 100 points nothing can be paid out at all, so step 2 replaces the option cards with a plain explanation and the number of points still needed.

## Investing cash

From step 1, "Invest cash" opens a second flow where she hands over real money and it becomes points at the same rate points are worth: **$1 buys 10 points**. The result card tells you to collect the cash and what her new board total is.

The screen shows what the money actually buys her, because that is the lesson:

| | |
|---|---|
| She hands over | $10.00 |
| She gets | 100 points |
| Her bonus every payout | $10.00 to $11.00 |
| Extra earned in 12 months | $12.00 |

It also flags inefficient amounts. At 1,028 points, investing $8 reaches 1,100 exactly and raises her bonus by the same $1 that $10 would, so the extra $2 buys points that do nothing until she reaches 1,200. Only points that complete a hundred do any work.

Right after a payout that pays cash, step 3 offers to put some of it back in, starting from her post payout balance rather than the pre payout one.

### Two rules worth knowing

**Invested points are for the next payout.** They land on the board and start earning at the following payout. If a kid invests and then runs a payout in the same sitting, step 2 shows a warning, because buying 100 points for $10 and immediately cashing them out for $11 would be free money.

**There is no deposit cap.** Every invested dollar returns 10 cents at every payout, permanently, and that obligation never shrinks. $100 invested is 1,000 points paying $10 a month forever, and she can still cash the whole thing out for $110. If that gets uncomfortable, a cap belongs in `refreshInv()` and `RULES`.

## Why Roll Over is worth picking

Roll Over and Bonus Only are worth the same amount today. At 1028 points, Bonus Only hands over $10 and Roll Over hands over 100 points, which are worth $10. The real difference is when the next raise arrives, so each option card says truthfully what happens to next month's bonus:

- Crossing a hundred: "Your monthly bonus goes up from $10.00 to $11.00, every month from now on."
- Not crossing: "Your bonus stays at $2.00 until you reach 300 points. You would need 30 more after this."

Starting at 290 points and rolling over every payout with no new points earned: 290, 310, 340, 370, 400, 440.

## Seeing it play out over time

Step 2 has a 6 month and 12 month toggle. Each option card shows a third number: the cash collected over that stretch plus what the board would be worth if cashed out at the end, assuming the same choice every payout and no new points earned.

Starting at 1,028 points:

| Option | 6 months | 12 months |
|---|---|---|
| Cash Out | $110.00 | $110.00 |
| Bonus Only | $170.00 | $230.00 |
| Roll Over | $187.00 | $341.00 |

The zero earning assumption is what makes the mechanic visible on its own, and it is stated on screen. It does make Cash Out look flat, since with no new points there is nothing to cash out after the first payout. Adding a "points earned each month" input to `project()` is the natural next step if that feels unfair.

## Notes

- Avery is pink with a unicorn, Rowan is purple with a dinosaur. Colors live in the `KIDS` object and the CSS variables at the top, each with a `dark` shade used for hover and pressed states.
- The action buttons are fixed to the bottom of the screen so they stay put while scrolling. Step 2 is built to fit an iPad without scrolling at all.
- The points field holds raw digits while it is being typed in and a comma formatted number the rest of the time, so the caret never jumps.
- Every payout is a whole dollar amount. Because only full hundreds convert, there are never any cents to count out.
- Each girl's ending balance is remembered in localStorage so step 1 pre-fills next month. If storage is unavailable the app still works, it just does not pre-fill.
- Saving still wins given enough time. At 1028 points, Bonus Only pays $10 a month and she keeps the whole balance.
