---
id: how-does-detective-hour-work
title: What is Detective Hour and how does it work?
sidebar_label: Detective Hours
---

## What is "Detective Hour"?

Detective Hour, or simply DH, is the unit used to measure the time The Detective spends watching your prints.

It is indicated by the symbol <img src="/img/user_guides/detective-hour-primary.png" alt="Detective Hour" style={{height: "1.3em", display: "inline", verticalAlign: "text-bottom"}}></img> in the system.

## Why do I need Detective Hours?

You need a positive balance of Detective Hours on your account to have The Detective watch your prints. [Here is why.](#why-do-you-make-it-so-complicated)

You do **NOT** need Detective Hours to watch your printer feed in the app. You can watch it for as long as you want. There is no limit to it!

## How do I get Detective Hours?

There are several ways to get Detective Hours:

1. You can earn Detective Hours by [helping The Detective get better](/docs/how-does-credits-work/).

1. If you have [the Free plan](/docs/upgrade-to-pro), you will get 10 Detective Hours for free each month.

1. You can buy or subscribe to one of the [Detective Hour packs](https://app.thespaghettidetective.com/ent_pub/pricing/#need-more). You can get 500 hours for just $20.

1. If you subscribe to [the Pro plan](/docs/upgrade-to-pro), you will receive 50 DHs as part of your subscription **each month**. You can add one of the [Subscribe&Save Detective Hour packs](https://app.thespaghettidetective.com/ent_pub/pricing/#need-more) to your Pro plan at a discounted rate.

## FAQs

#### Where can I check the DH balance on my account?

There is an icon <img src="/img/user_guides/detective-hour-primary.png" alt="Detective Hour" style={{height: "1.3em", display: "inline", verticalAlign: "text-bottom"}}></img> at the top of the page that shows the current DH balance on your account. You can click on that icon and see this balance broken down to the DHs included in your plan and the DHs you earned or bought.

![DH nav bar](/img/user_guides/helpdocs/dh-balance-navbar.png)

#### What will happen if the DH balance on my account goes to 0?

When your Detective Hours run out, The Detective will stop watching your prints, and you won't get alerted even if your prints start to fail. Everything else remains the same. You can still start, pause, or cancel your prints in OctoPrint or The Spaghetti Detective. Your webcam feed will stream as usual too.

:::note
You can still watch your printer feed in the app even if your DH balance goes to 0.
:::

#### I heard I can buy more DHs. Is it true?

Yes. Additional DHs can be bought at $20 for 500 hours, or $0.04/hour. If you have subscribed to the Pro plan, you can also add one of the [Subscribe&Save DH packs](https://app.thespaghettidetective.com/ent_pub/pricing/#need-more) to your Pro plan at a much lower rate.

Go to [your subscription page](https://app.thespaghettidetective.com/ent_pub/pricing/#need-more) to buy more DHs.

#### 10 FREE DHs per month for free account is not enough!

[It costs non-trivial amount to watch every hour of print](/docs/how-does-detective-hour-work#why-do-you-make-it-so-complicated) and therefore we can't afford to give more to users on the free plan. :(
  
Even if 10 DHs per month is not a lot, if you turn on detection only for your "risky prints", it can still significantly reduce your stress level, and sometimes save your printer from a jammed heater block or fan. 

#### Tell me more about the DHs that are included in the Pro plan.

If you subscribe to the Pro plan, you will get 50 DHs **every month**. If not all 50 DHs are used when the month ends, the unused DHs [roll over into next month](#tell-me-exactly-how-dhs-roll-over-works), as long as you keep your subscription active. The same roll-over rule applies to the DHs that comes from the Subscribe&Save DH Packs.

#### What happens to the balance of my DHs when I cancel my subscription plan?

If you cancel your subscription plan, the remaining DHs that originally came from the Pro plan or the Subscribe&Save DH pack will expire at the time when the plan ends.

You will get to keep the DHs that you bought in a Pre-paid DH pack, or earned by contributing to The Detective's learning.

#### Is there an easy way to tell The Detective not to watch my prints when I don't want to use DHs?

Yes! We have redesigned our printer page to make it super easy! We have also added visual cue so that it's clear when you are not using your hard-earned DHs.

![How to disable watching](/img/user_guides/helpdocs/disable_watching.png)

#### Hey my DH balance shows a negative number. Is The Detective out of her mind?

No. The Detective is just being extra nice.

As long as you have a positive DH balance when a print starts, The Detective will watch your print and she will **not** stop watching mid-print just because your DH balance reaches 0. This is to make sure you wouldn't be surprised that you were not covered where you thought you were.

Suppose you only have 1 DH when you kick off a 10-hour print. The Detective will still watch the whole 10-hour even if you are already in the red zone after 1 hour. However, after the print is done, your DH balance will be -9.

Your DH balance will be filled up to a positive number at the time when you [get more DHs](#how-do-i-get-detective-hours).

#### Why do you make it so complicated?

The reason why we took the trouble to properly account for DHs is because every single <img src="/img/user_guides/detective-hour-primary.png" alt="Detective Hour" style={{height: "1.3em", display: "inline", verticalAlign: "text-bottom"}}></img> costs computational resources.

When The Detective is actively watching your print, the TSD server continuously analyzes the pictures sent from your webcam. This analysis, known as [Deep Learning](https://en.wikipedia.org/wiki/Deep_learning), uses an expensive resource called [GPU](https://en.wikipedia.org/wiki/Graphics_processing_unit).

This is exactly why we took the trouble to program The Detective so that she doesn't spend any time watching when you are not printing, or don't want her to watch your prints.

Also by accounting for the usage of DHs, we want to motivate everyone to optimize how we use our DHs. Only by doing so can we provide this service to the entire 3D printing community at affordable cost.


## Not-So-Frequently-Asked Questions

#### Tell me exactly how DHs are accounted for.

*1 DH = 1 hour worth of time when your printer is printing AND The Detective is actively watching your print.*

This means the time when your printing is idle (not printing) won't be counted as DHs. Also, when you disable detection for a print, or for a period in a print, that period will not be counted as DHs either.

Let's use an example to illustrate how Detective Hours are accounted for:

| Start  | End    | Activity                                                                                                | Duration   | Is DH? | Explanation                        |
|--------|--------|---------------------------------------------------------------------------------------------------------|------------|--------|------------------------------------|
| 6am    | 6:15am | You turn on the 3D printer. Then you prepare the G-Code and the print bed                               | 0.25 hours | No     | Not DH when printer is idle.       |
| 6:15am | 7am    | You start the print. Your printer prints happily.                                                       | 0.75 hours | Yes    |                                    |
| 7am    | 7am    | The Detective sends you an alert. It's a false alarm caused by gyroid infill. So you disable detection. | 0 hours    |        |                                    |
| 7am    | 8:30am | Your printer prints happily and finishes the print.                                                     | 1.5 hours  | No     | Not DH when detection is disabled. |
| 8:30am | 5pm    | Your printer is left on when you are at work. You turn it off after you come home.                      | 8.5 hours  | No     |                                    |

In this example, only 0.75 DHs are accounted for, even if the print lasts 2.25 hours, and the printer is left on for 11 hours.

#### Tell me exactly how DH's roll-over works.

First of all, there are 2 different kinds of DHs.

1. The DHs that are included in your subscription plan. For instance, the Free plan includes 10 DHs per month; Pro plan includes 50 DHs per month. There are also "Subscribe&Save DH Packs" that include various number of DHs each month. Let's call them *included DH*s.
2. The DHs that you buy in a Pre-paid DH pack, or otherwise earn by teaching The Detective. Let's call them *earned DH*s.

Here are the complete list of all roll-over rules on DHs:

- The balance of *included DH*s roll over into next month, as long as the subscription is kept active. The roll-over happens when the next billing cycle is fully paid for.
- The balance of *earned DH*s roll over infinitely. It doesn't matter if you have a subscription plan at all.
- To minimize the amount of *included DH*s you would loose should you cancel the subscription plan, *included DH*s are always used before *earned DH*s.

#### A complete example to show how Detective Hour works.

| Date      | Activity                                                                | Earned DHs | Included DHs | Total DH |
|-----------|-------------------------------------------------------------------------|------------|--------------|----------|
| 1/1/2020  | Earns 20 DHs by uploading 20 time-lapses                                | 20         | 0            | 20       |
| 1/5/2020  | Prints 10 hours                                                         | 10         | 0            | 10       |
| 1/10/2020 | Upgrades 2 printers to Pro plan                                         | 10         | 50           | 60       |
| 1/15/2020 | Prints 24 hours, with detecting disabled for 4 hours                    | 10         | 30           | 40       |
| 1/20/2020 | Earns 25 DHs by uploading 25 time-lapses                                | 35         | 30           | 65       |
| 1/25/2020 | Prints 40 hours (2 printer total), with detection disabled for 2 hours  | 27         | 0            | 27       |
| 2/10/2020 | Renews Pro plan                                                         | 27         | 50           | 77       |
| 2/15/2020 | Cancels Pro plan                                                        | 27         | 0            | 27       |
