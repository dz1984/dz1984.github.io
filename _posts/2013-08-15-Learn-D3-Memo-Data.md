---
layout: post
title: 學習D3筆記 - 讀取資料(草稿)
categories: articles
tags: [D3,javascript,Memo]
image:
  feature: texture-feature-01.jpg
date: 2013-08-15 20:30
---
分別可以用d3.csv()和d3.json()讀取CSV及JSON檔案。下面就拿d3.csv()為例子，d3.json()同d3.csv()操作。 (若是用Tab為分隔，要改用d3.tsv() )

{% highlight js %}
d3.csv('food.csv',function(data){
	console.log(data);
});
{% endhighlight %}
加入error參數至callback函數。若error有訊息，那data會是undefined，反之，error為null，data就會有資料。
{% highlight js %}
var dataset;

d3.csv('food.csv',function(error,data){
	if (error){
		console.log(error);
	} else{
		console.log(data);
	}
	dataset = data;
	generateVis();
	hideLoadingMsg();
});
{% endhighlight %}
使用enter()去建立新的data-bound元素，會將資料塞給目前DOM物件。
{% highlight js  %}
var dataset = [5,10,15,20,25];

d3.select("body").selectAll("p")
	.data(dataset)
	.enter()
	.append("p")
	.text("New paragraph!");
	
/* OUTPUT
New paragraph!

New paragraph!

New paragraph!

New paragraph!

New paragraph!
*/
{% endhighlight %}
現在試著讀取dataset資料至頁面。
{% highlight js %}
var dataset = [5,10,15,20,25];

d3.select("body").selectAll("p")
	.data(dataset)
	.enter()
	.append("p")
	.text(function(d,index){
		return 'Dataset['+index+']:'+d;
	});
	
/* OUTPUT:
Dataset[0]:5

Dataset[1]:10

Dataset[2]:15

Dataset[3]:20

Dataset[4]:25
*/
{% endhighlight %}
加點顏色上去。
{% highlight js %}
var dataset = [5,10,15,20,25];

d3.select("body").selectAll("p")
	.data(dataset)
	.enter()
	.append("p")
	.text(function(d,index){
		return 'Dataset['+index+']:'+d;
	})
	.style("color",function(d){ 
		if (d<15) 
			return 'red';
		else
			return 'blue';
	});
/* OUTPUT
Dataset[0]:5(R)

Dataset[1]:10(R)

Dataset[2]:15(B)

Dataset[3]:20(B)

Dataset[4]:25(B)
*/
{% endhighlight %}
