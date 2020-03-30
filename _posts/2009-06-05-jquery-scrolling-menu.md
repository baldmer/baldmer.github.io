---
layout: post
title: JQuery Scrolling Menu
tags: [javascript]
---

A lot of people have been wondering how to code a "Scrolling Menu" or animated menu (like in the categories section on my web log). Here are a couple of `JQuery` code samples to achieve that fancy behavior.

{% highlight javascript linenos %}
$(document).ready(
	function(){
		$(window).scroll(function(){
			$('.sidebar').stop();
			var scroll = $(window).scrollTop();
			var menuPosition = 0;
			if (scroll > 370)
			   menuPosition = scroll - 210;
			else
			   menuPosition = 0;
			$('.sidebar').animate({top: menuPosition},'fast');
		});
	}
);
{% endhighlight %}

Since the above code snippet consumes a lot of CPU resources, in the following optimization the menu animates on the window's `mouseup` event instead of every time you scroll the window.

{% highlight javascript linenos %}
$(document).ready(
	function(){
		var scroll = $(window).scrollTop();
		var menuPosition;
		/*
			if the user clicks a link to other page
			and then go back, the menu must be setted to
			its current position.
		*/
		if (scroll > 370){
			menuPosition = scroll - 210;
			$('.sidebar').animate({top: menuPosition},'slow');
		}

		$(window).scroll(function(){
			$('.sidebar').stop();
			scroll = $(window).scrollTop();
			if (scroll > 370)
			  menuPosition = scroll - 210;
			else
			  menuPosition = 0;
		});

		$('body').mouseenter(function(){
      			$('.sidebar').animate({top: menuPosition},'slow');
    		}).click(function(){
      			$('.sidebar').animate({top: menuPosition},'slow');
    		});
		$(window).mouseup(function(){
      			$('.sidebar').animate({top: menuPosition},'slow');
    		});
	}
);
{% endhighlight %}


Another events must be fired to achieve the same behavior on IE or devices like a mouse with scrolling button, touchpad, etc.. For example, on IE when the user hold the scroll bar and moves up/down, the menu must be animated when the `mouseenter` event of the page's body is fired. On a mouse with scrolling button the same behavior can be reached on the `click` event of the page's body.

