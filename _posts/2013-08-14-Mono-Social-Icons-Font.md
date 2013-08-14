---
layout: post
title:	Mono Social Icons Fonts 
categories: articles
tags: [font,icon,jekyll,Bootstrap]
image:
  feature: texture-feature-01.jpg
---
今天編輯Blog時，打算將所有知名使用過的社交網站連結點合併，就像左手邊看到的Biography。原本[Minimal Mistakes][1]版本是用[IcoMoon][2]字型，但因為套用[Bootstrap][4]後，與原本樣式有衝突，索性全改成[Bootstrap][4]，以後也比較好維護。所以~就將author-bio.html裡面內容，改成[Mono Soical Icons Font][3]版本，並加上Blogger和About.me連結上去。

說到套用[Mono Soical Icons Font][3]一點也不難，就在CSS裡增加一組字型，並多設個symbol的class樣式，而HTML輕輕鬆鬆地加入symbol class，並在title屬性告知要用那個icon就行了！
<script src="https://gist.github.com/DonaldIsFreak/8f4a9b17ce2e5b1ca0b6.js"></script>


[1]:http://mmistakes.github.io/minimal-mistakes
[2]:http://icomoon.io
[3]:http://drinchev.github.io/monosocialiconsfont/
[4]:http://getbootstrap.com/