# Family Payday

A three step tool for the monthly points payout. Built for a kid on the iPad.

Open `index.html` by double clicking it. No server needed.

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
