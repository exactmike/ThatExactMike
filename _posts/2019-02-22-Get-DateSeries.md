---
title: "Get-DateSeries"
excerpt: "gets a series of datetime objects for a specified start, interval, interval units, and instance limit."
date: 2019-02-27
last_modified_at: 2019-02-27
comments: true
tags:
  - PowerShell
  - DateTime
  - DateTime Series
  - Function
---

# background

I needed to schedule a series of meetings and wanted to know/review all the dates and times before sending the invitation.  Outlook, as far as I know, doesn't make this very easy to do, and I can imagine many other scenarios (custom timers or Pomodoro timers?) where someone might want to use this.

# feedback

I haven't searched a lot yet, but is there an existing community module that does other interesting things with DateTime?  Would love to contribute this there if so.

# code

{% gist 517c5005319952c785a191c6143ca467 Get-DateSeries.ps1 %}
