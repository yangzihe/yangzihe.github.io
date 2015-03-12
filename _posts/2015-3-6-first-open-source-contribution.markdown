---
layout: post-header
title: "My First Open Source Contribution"
tags: [Reddit Enhancement Suite, Javascript, Open Source]
---

Pretty happy with myself as I just finished making my first open source contribution ever! Reddit is a website that I frequent a lot, and [RES](http://redditenhancementsuite.com/) makes Reddit so much more convenient to use, so I thought it would be really cool to contribute to something that I, myself, am using.

The problem in this situation was that user account ages were being calculated incorrectly. A user who registered on the site on February 21st, 2015 was shown to have an account age of 16 days when viewed on March 3rd, 2015. The correct age---not including the end date---is 13 days.

After seeing the bug in a user's profile first hand, I immediately guessed that it had something to do with February and how it only has 28 days in it. This was further reinforced by the fact that the user's account age was shown to be 3 days older, which is the difference in days between 31 and 28. The basic logic of the date structure was to subtract dates piecewise---year minus year, month minus month, day minus day. To handle instances where not an entire year/month passed by, the function would check whether or not the difference months or days were negative. If the difference in months was negative, then it would add 12 to that number, and subtract one from the difference in years. The same thing happens for days, except the number of days to add varied between 28-31. Ahhhh. After running through the code with examples on paper a couple times, I was pretty confident I had found the bug in the logic. There was an array that stored the number of days in each month, and code looked like this

~~~~~
    if (leapYear) {
        var f = 28;
    } else {
        var f = 29;
    }
    var m = [31, f, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
    if (dday < 0) {
        var ma = amonth + tmonth;
    }
    if (ma > 12) {
        ma = ma - 12;
    }
    if (ma < 0) {
        ma = ma + 12;
    }
    dday = dday + m[ma];
~~~~~

I didn't understand the logic behind the original code for accounting for partial months, so I thought about how I would implement it, and I figured I would replace it with my logic. So, given two dates in the same year, such as 2-21 and 3-6, how would I calculate the difference using the paradigm given above? My first thought was that if both dates were in the same month, then it would be really easy. We would just subtract the later date from the earlier one and get the difference in days that we want. But we don't have the later date in a format we want. How do I fix that? Simple: convert it.

To convert the later date into the same month as the earlier date, just take the last day in the month of the earlier date, and then add the leftover days from the later month to it. In this case, we would do the calculation `dday = 28 + 6 - 21`, which gives us 13 days---the correct solution. As such, I immediately thought, we should be setting `ma` to `amonth`---the variable for the month that the user originally registered. There! All done! Easy peasy. Or so I thought.

I was wrong. When I went to test my implementation against the old one using my own account age, I found out that I got three different durations: one from my implementation, one from the old implementation, and a third from date difference calculators online. Uh oh.

The immediate bug that I found running through test calculations by hand was that I had made an error in my assumptions. I had assumed one was supposed to access the total of number of days in the month of registration. But when I thought about it, assuming the difference in months was greater than one, and the day in the later month is smaller than the day in the earlier month, to calculate the date difference, there are two parts. One, increment the months of the earlier date as much as possible while not passing the later date. That's the total number of whole months that have passed. Then use the formula I described in earlier paragraphs. So instead of accessing the number of days in the month of registration, I was supposed to access the number of days in the month before the current date. It just so happened that in the case I encountered, the month before the current date's month was the month of registration, and it made me come to the incorrect solution.

So now I fixed that bug, but I still had one remaining. Although the online date calculator agreed with my implementation regarding the total number of days passed, it stated the duration as "2 years, 4 months" while my implementation stated "2 years, 3 months, and 28 days". For the sake of reference, my registration date was **November 6th, 2012** and the date I was debugging the issue was **March 6th, 2015**. At first I thought it had something to do with February again, since the number of days was 28. I tried my technique of incrementing the year as much as possible, and then looking at the total number of days left over and figuring out how they implemented counting months. Doing so left me with a partial rest of the month in November, all of December, January, and February, and a partial month in March. This got me thinking about how the date calculator dealth with partial months. Since I had partial months in November and March, how many days does the date calculator count as 1 month? If the days added up to 28? 30? 31? Was it absolute or did it depend on particular months? At that point my brain was twisting in circles so I decided to stop for a bit and come back later.

When I returned to this problem, I went back to the basics. What are the invariants when calculating the difference between two dates? Entire units. Because regardless of whether the year is a leap year, or any other of the countless factors one must consider when figuring out the date, what doesn't change, and what shouldn't change is going from a particular day in a particular month from one year to the same day and month in the next year is a month. Similarly, if you go from a certain day in a month to the same day in the next month, that duration is 1 month. Even though one could argue that implementation isn't technically correct, I would argue it's the most human/understandable one, as if you were to ask a person the difference between the two aforementioned types of dates, they would undoubtedly say 1 year and 1 month respectively.

Thus, that was how I decided to rewrite the logic for the date difference calculator. First, increment the year as many times as possible while not surpassing the current date. Then do the same for the month. After figuring out the year and month, I would use the previously mentioned method to calculate the difference in days, and subtract the number of years/months accordingly.