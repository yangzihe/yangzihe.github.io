---
layout: post-header
title: "My First Open Source Contribution"
tags: [Reddit Enhancement Suite, Javascript, Open Source]
permalink: /blog/:title.html
---

Pretty happy with myself as I just finished making my first open source contribution ever! Reddit is a website that I frequent a lot, and [RES](http://redditenhancementsuite.com/) makes Reddit so much more convenient to use, so I thought it would be really cool to contribute to something that I, myself, am using.

The problem in this situation was that user account ages were being calculated incorrectly. A user who registered on the site on February 21st, 2015 was shown to have an account age of 16 days when viewed on March 3rd, 2015. The correct age---not including the end date---is 13 days.

After seeing the bug in a user's profile first hand, I immediately guessed that it had something to do with February and how it only has 28 days in it. This was further reinforced by the fact that the user's account age was shown to be 3 days older, which is the difference in days between 31 and 28. The basic logic of the date structure was to subtract dates piecewise---year minus year, month minus month, day minus day. To handle instances where not an entire year/month passed by, the function would check whether or not the difference months or days were negative. If the difference in months was negative, then it would add 12 to that number, and subtract one from the difference in years. The same thing happens for days, except the number of days to add varied between 28-31. Ahhhh. After running through the code with examples on paper a couple times, I was pretty confident I had found the bug in the logic. There was an array that stored the number of days in each month, and code looked like this

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

I didn't understand the logic behind the original code for accounting for partial months, so I thought about how I would implement it, and I figured I would replace it with my logic. So, given two dates in the same year, such as 2-21 and 3-6, how would I calculate the difference using the paradigm given above? My first thought was that if both dates were in the same month, then it would be really easy. We would just subtract the later date from the earlier one and get the difference in days that we want. But we don't have the later date in a format we want. How do I fix that? Simple: convert it.

To convert the later date into the same month as the earlier date, just take the last day in the month of the earlier date, and then add the leftover days from the later month to it. In this case, we would do the calculation `dday = 28 + 6 - 21`, which gives us 13 days---the correct solution. As such, I immediately thought, we should be setting `ma` to `amonth`---the variable for the month that the user originally registered. There! All done! Easy peasy. Or so I thought.