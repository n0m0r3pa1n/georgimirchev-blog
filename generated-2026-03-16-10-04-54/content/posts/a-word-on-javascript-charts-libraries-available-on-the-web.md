---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:25"
_publicize_job_id: "43825108220"
author: gmirchev90
categories:
  - web
date: "2020-05-04T13:48:28+00:00"
guid: http://georgimirchev.com/?p=193
parent_post_id: null
post_id: "193"
summary: |-
  I had to make a project which contained a lot of charts and very different ones. I needed charts like:

  - Pie chart
  - Column chart
  - Multiple column chart with 2 different y axis
  - Column & line chart combined together
  - Column & line chart combined with 2 different y axis
tags:
  - charts
  - javascript
timeline_notification: "1588600109"
title: A word on Javascript charts libraries available on the web
url: /2020/05/04/a-word-on-javascript-charts-libraries-available-on-the-web/

---
I had to make a project which contained a lot of charts and very different ones. I needed charts like:

- Pie chart
- Column chart
- Multiple column chart with 2 different y axis
- Column & line chart combined together
- Column & line chart combined with 2 different y axis

My requirements for these charts were to be mobile friendly and to have the 2 different Y axis option. It is very important.

I tried:

- [Google Charts](https://developers.google.com/chart/)
- [Chart.JS](http://chartjs.org/)
- [HighCharts](http://highcharts.com/demo/combo)

Let’s keep it short. The winner is: **HighCharts**. It is mobile friendly, it allows options like zoom, removing the visible columns by toggling their label, it scales great, allows export to PDF, PNG etc. and is highly customizable. A GREAT LIBRARY!

Others are either buggy, miss the 2 different y axis option, even google charts have some bugs with their new material charts. Forget about the other two and use HighCharts. What an advertisement I have made, but they really made my day!
