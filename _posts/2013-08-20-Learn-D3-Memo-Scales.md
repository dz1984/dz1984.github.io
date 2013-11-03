---
layout: post
title: 學習D3筆記 - 刻度(草稿)
categories: articles
tags: [D3,Javascript,Memo]
image:
  feature: texture-feature-01.jpg
date: 2013-08-20 22:33
---
### 線性刻度
只要定義好資料集最小值和最大值的**Domain**，以及要設定**Range**長度就能做刻度上轉換，用法也很簡單。
{% highlight js %}
var dataset = [20,30,50,70,100,200,1000];
var scale = d3.scale.linear()
	.domain([
		// Catch min and max of dataset.
		d3.min(dataset,function(d){return d; }),
		d3.max(dataset, function(d){ return d; })
	])
	.range([0,100]);
	
console.log(scale(500));	// Print 48.9795918367347
{% endhighlight %}

現在把它放進之前寫的散佈圖裡。  
{% highlight js %}
/*
	Generate random function.
	rand(max) - generate a random number between zero and max.
	rand(min,max) - generate a random number between min and max.
	rand(min,max,n) - genrate n random numbers between min and max.
*/
var rand = function(){
	var n=1, min=0, max=9999, argc = arguments.length;
	
	if (argc>3)
		return -1;
		
	switch(argc){
		case 1:
			max = arguments[0];
			break;
		case 3:
			n = arguments[2];
		case 2:
			min = arguments[0];
			max = arguments[1] - min;
			break;
	}
	// Closure function
	var _rand = function(){
		return Math.round(Math.random()* max+min);
	};
	
	if (n==1)
		return _rand();
	else{
		result = [];
		for (var i=0; i<n; i++)
			result.push(_rand());
		
		return result;
	}
};

var dataset = [];
for (var i=0; i<10; i++)
	dataset.push(rand(5,30,2));

var w=500,h=500,padding=20;

// Use the linear scales, and I set the padding on the paint area.
var xScale = d3.scale.linear()
	.domain([0,d3.max(dataset, function(d){ return d[0]; })])
	.range([padding,w-padding*2]);

var yScale = d3.scale.linear()
	.domain([0,d3.max(dataset, function(d){ return d[1];})])
	.range([h-padding*2,padding]);

var svg = d3.select('body')
	.append('svg')
	.attr('width',w)
	.attr('height',h);

var circles = svg.selectAll('circle')
	.data(dataset)
	.enter()
	.append('circle');

// Adjust the cx,cy of Circle.
circles.attr({
	cx:		function(d){ return xScale(d[0]); },
	cy:		function(d){ return yScale(d[1]); },
	r:		function(d){ return Math.sqrt((d[0]*d[1])/3.14); },
	"fill":	"green"
});

var labels = svg.selectAll('text')
	.data(dataset)
	.enter()
	.append('text')
	.text(function(d) { return d[0]+","+d[1]; });

labels.attr({
	x:		function(d){ return xScale(d[0])+10; },
	y:		function(d){ return yScale(d[1]); },
	"font-family":	"sans-serif",
	"font-size":	"11px",
	"fill":		"red"
});
{% endhighlight %}
[Demo](http://bit.ly/18ILD6b)
### 線性刻度其他method
* nice()
* rangeRound()
* clamp()

#### 參考資料
1. [Interactive Data Visualization for the Web-Ch.7](http://bit.ly/12nBOK2)