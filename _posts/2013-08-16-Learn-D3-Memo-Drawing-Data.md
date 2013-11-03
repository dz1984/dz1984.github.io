---
layout: post
title: 學習D3筆記 - 繪圖數據(草稿)
categories: articles
tags: [D3,Javascript,Memo]
image:
  feature: texture-feature-01.jpg
date: 2013-08-16 09:48
---
### 隨機長條圖
長條圖的CSS styles。
{% highlight css  %}
div.bar {
	display: inline-block;
	width: 20px;
	height: 75px;
	background-color: red;
	margin-right: 2px;
}
{% endhighlight %}

使用.attr()將所有資料套入長條圖的CSS styles，另外可用.classed()快速套用或移除元素樣式。。
{% highlight js %}
var dataset = [5,10,15,20,25];

d3.select("body").selectAll("p")
	.data(dataset)
	.enter()
	.append("div")
	.attr('class','bar')
	.classed('bar',function(d){
		if (d>15)
			return true;
		else
			return false;
	});
{% endhighlight %}
利用.style()來完成條樣的長條圖吧！
{% highlight js %}
var dataset = [5,10,15,20,25];

d3.select("body").selectAll("p")
	.data(dataset)
	.enter()
	.append("div")
	.attr('class','bar')
	.style('height',function(d){
		var barHeight = d*5;
		return barHeight+'px'
	});
{% endhighlight %}
讓資料來點隨機性，看看有什麼事情發生？一堆不規則的長條圖。
{% highlight js %}
var dataset = [];

for (var i=0; i<25; i++)
	dataset.push(Math.round(Math.random()*30));

d3.select("body").selectAll("p")
	.data(dataset)
	.enter()
	.append("div")
	.attr('class','bar')
	.style('height',function(d){
		var barHeight = d*5;
		return barHeight+'px'
	});
{% endhighlight %}
[Demo](http://bit.ly/17IF1ns)

### 使用SVG繪畫圓點
CSS繪圖能力有限，所以~改用SVG來畫圖，先新增一個SVG元素插入body。
{% highlight js %}
// Create a new SVG instance.
var w=800,h=600;

var svg = d3.select('body')
	.append('svg')
	.attr('width',w)
	.attr('height',h);
{% endhighlight %}
透過data-bound方式，在SVG元素產生的圓形物件。
{% highlight js %}
var circles = svg.selectAll('circle')
	.data(dataset)
	.enter()
	.append('circle');
{% endhighlight %}
最後，幫這些小圓點上妝補色。
{% highlight js %}
circles.attr('cx',function(d,i){
	return (i*60)+30;
})
.attr('cy',h/2)
.attr('r',function(d){
	return d;
})
.attr('fill','red')
.attr('stroke','green')
.attr('stroke-width',function(d){
	return d*0.2;
});
{% endhighlight %}
[Demo](http://bit.ly/16JcrWp)

### SVG版長條圖
把原本CSS版長條圖改成SVG版本，先建立SVG物件。
{% highlight js %}
// Create a new SVG instance.
var w=300,h=200,barPadding=1;

var svg = d3.select('body')
	.append('svg')
	.attr('width',w)
	.attr('height',h);
{% endhighlight %}
加入長條圖形的SVG tag，調整長條之間位置和長度。。
{% highlight js %}
var rects = svg.selectAll('rect')
	.data(dataset)
	.enter()
	.append('rect');

var barW = w/dataset.length-barPadding,barScale=4;

rects.attr({
		width: barW,
		height: function(d){ return d*barScale;	},
		x: function(d,i){ return i*(w/dataset.length); },
		y: function(d){ return h-d*barScale; },
		fill: function(d) { return d3.rgb(d+200,d+200,d+200); }
	});
{% endhighlight %}
加入資料Label。
{% highlight js %}
var labels = svg.selectAll('text')
	.data(dataset)
	.enter()
	.append('text')
	.text(function(d){ return d; });

labels.attr({
	x: function(d,i) { return i*(w/dataset.length)+barW/2;},
	y: function(d,i) { return h-d*barScale-5; },
	"font-size": "11px",
	"text-anchor":"middle"
});
{% endhighlight %}
[Demo](http://bit.ly/13PYtMB)

### 畫個散佈圖
要畫散佈圖一定要有x,y的座標資料，所以~我改寫亂數函數，可以調整參數一次產生幾個亂數。
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
	};
	// Closure function
	var _rand = function(){
		return Math.round(Math.random()* max+min);
	}
	
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

// Generate ten points that (x,y) is between 5 and 30.
for (var i=0; i<10; i++)
	dataset.push(rand(5,30,2));
{% endhighlight %}
加入圓點。
{% highlight js %}
var circles = svg.selectAll('circle')
	.data(dataset)
	.enter()
	.append('circle');

circles.attr({
	cx:		function(d){ return d[0]*10; },
	cy:		function(d){ return d[1]*10; },
	r:		function(d){ return Math.sqrt((d[0]*d[1])/3.14); },
	"fill":	"green"
});
{% endhighlight %}
加入各點的座標。
{% highlight js %}
var labels = svg.selectAll('text')
	.data(dataset)
	.enter()
	.append('text')
	.text(function(d) { return d[0]+","+d[1]; });

labels.attr({
	x:		function(d){ return d[0]*10+10; },
	y:		function(d){ return d[1]*10; },
	"font-family":	"sans-serif",
	"font-size":	"11px",
	"fill":			"red"
});
{% endhighlight %}
[Demo](http://jsbin.com/eRaYOPO/)

#### 參考資料
1. [Interactive Data Visualization for the Web-Ch.6](http://bit.ly/14AnxY9)