> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [medium.com](https://medium.com/@zkareemz/golden-ratio-62b3b6d4282a)

> For the past few days I had this question in my mid “what is the best line height value for 14px font......

For the past few days I had this question in my mid  
“**what is the best line height value for 14px font size on my screen?**”,  
I start digging everywhere on the web to know how people calculate it, and I got some interesting numbers:

Twitter [bootstrap](http://getbootstrap.com/) is using a ([**1.42857**](https://github.com/twbs/bootstrap/blob/master/less/variables.less#L63)**)** as a ratio between the font size and the line height (line-height = font-size * 1.42857).  
HTML5 [boilerplate](http://html5boilerplate.com/) using almost the same value which is (**1.4**).

[Medium](https://medium.com/) and zurb [foundation](http://foundation.zurb.com/) framework they are using the same ratio for the line height and it is (**1.5**).

And just for your information adobe [topcoat](http://topcoat.io/) is using ([**1.313**](https://github.com/topcoat/topcoat/blob/master/css/topcoat-desktop-dark.css#L170)**)** and yahoo [purecss](http://purecss.io/) is using (**1.58**), so which one should I use?

> Note that 1.5 is a value that is commonly recommended in classic typographic books, so our study backs up this rule of thumb. :[smashing magazine](http://www.smashingmagazine.com/2009/08/20/typographic-design-survey-best-practices-from-the-best-blogs/)

Personally I don't know which one to use, is it old industry numbers or unknown magical numbers? can I create my own number?

The nature gave us an incredible proportion and it is around us everywhere known as the [golden ratio](http://en.wikipedia.org/wiki/Golden_ratio) (1.618). So I will use ( 14px * 1.618 ) as my line height, ok I know the answer to my first question but another question just jumped to my head  
**“what is the best line width for 14px font size and 23px line height?**”

So how I can figure out the a relation between the font size and the line width? all what I know here is the line width is significantly greater than the line height, and thats a good start for me.

After reading some researches about the best [CPL](http://psychology.wichita.edu/surl/usabilitynews/72/pdf/Usability%20News%2072%20-%20Shaikh.pdf) (character per line) I found that 60%+ of people reading faster with 70 CPL and here you are the summery:

> Medium line length of 55 cpl for ease of reading, better comprehension and better reading rates for on-screen text compared to lines of 25 cpl or lOO cpl.
> 
> Optimal line width for a single column of text ranges from 45 to 75 cpl. For multiple columns, the line length should be 30 to 50 cpl.
> 
> Line length should be around 30 times (between 20 to 40) the size of the font type
> 
> Readers prefer short lines of about 8 to 10 words or 45 to 60 characters long

There is no fixed value here for the line width, its something between  
(30 cpl — 90 cpl) or (8 words— 12 words).

lets do the final step by calculating the line width:  
line width = average character width * cpl  
average character width = font size / character constant (**)

So finally:  
font size = 14px  
line height = (14***1.618**) = 23px  
width @ 50cpl = 50*(14/**1.618**) = 433px

I created [ [this](http://jsbin.com/todidu/1/edit?output) ] demo so you can try different line-height and line-width combinations.