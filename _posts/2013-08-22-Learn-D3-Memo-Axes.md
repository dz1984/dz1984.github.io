---
layout: post
title: 學習D3筆記 - 軸線(草稿)
categories: articles
tags: [D3,Javascript,Memo]
image:
  feature: texture-feature-01.jpg
date: 2013-08-22 22:20
---
### 增加X軸
SVG物件插入一個g元素，用D3的call()去建立X軸。
{% highlight js %}
var xAxis = d3.svg.axis()
	.scale(xScale)
	.orient("bottom")
	.ticks(5);
	
svg.append('g')
	.attr('class','axis')
	.attr('transform','translate(0,'+(h-padding)+')')
	.call(xAxis);
{% endhighlight %}