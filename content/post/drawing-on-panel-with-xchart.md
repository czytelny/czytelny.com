+++
date = "2013-09-06T22:03:11+01:00"
tags = ["Java SE"]
title = "Drawing chart on Swing Panel with XChart"

+++

The XChart (http://xeiam.com/xchart.jsp) is an easy-to-use lightweight and opensource library for drawing charts. It allows to create a really magnificent charts within minutes.

The official documentation is rich in examples how to create fancy charts, however the most obvious thing is hidden in demos:) so I decided to write this short info for beginners:)

The below line creates and displays new window which contains chart.

```java
new SwingWrapper(chart).displayChart();
```

If you have an existing plain swing panel (let's say ```pnlChartArea```) and want to draw a chart on it:

Set layout on pnlChartArea (for example box layout) ```right_click->set layout->box layout```
In the source code create new XChartPanel, add it into existing pnlChartArea, and most important, run ```validate()``` method to display it properly.

```java
JPanel pnlChart = new XChartPanel(chart);
this.pnlChartArea.add(pnlChart);
pnlChartArea.validate();
```
voila!