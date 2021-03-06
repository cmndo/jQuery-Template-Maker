jQuery Template Maker
========================

A small bit of logic for controlling a template by accessing element's node values and click events.


The function's name is **TemplateMaker()**. It takes 2 arguments.

*stringTemplate* - String - A precached, string version of HTML.

*contentObject* - Object - The model and controller grouped together. It holds the values that will overwrite `{{mustache}}` tags.

Click Events
---
This is helpful. Any element that you want to register a mouse click event on, give it a `on-click` data attribute and specify the property name of a function you add to *contentObject*. Here's an example.


HTML

    <script type="text/template" id="sectionTemplate">
       <section>
         <h3>{{title}}</h3>
         <p>{{copy}}</p>
         <a href="#" on-click="clickHandler">{{cta}}</a>
       </section>
    </script>

 
JavaScript

    var sectionTemplate = document.getElementById('sectionTemplate').innerHTML;	
    
    var section1 = TemplateMaker(sectionTemplate, {
	  title: "When Robots Take Over",
	  copy: "This book takes place in the near future. It is told from the perspective of a Facebook Oculus Rift developer working on a site project that ultimately leads to the misuse of virual spacial aware autonomous transportation units take over",
	  clickHandler: function(e){
	    alert("When Robots take Over was added to your cart.");
	  },
	  cta: "Buy Now"	          
	});
    
    $('body').append(section1);

[JSBin Example](http://jsbin.com/rokini/28/edit)

Update Your Template With New Content
---

When you make a template what you get is a jQuery Element. It's ordinary **except** It has 1 handy method added to it.

*update* - is a function - Used it to update the template with a new `contentObject`

**Example**

I'm going to build a simple content rotator. Lets start with the content.


    var rotatorContent = [
	  {
	    title: "When Robots Take Over",
	    copy: "This book takes place in the near future. It is told from the perspective of a Facebook Oculus Rift developer working on a site project that ultimately leads to the misuse of virual spacial aware autonomous transportation units take over",
	    clickHandler: function(e){
	      console.log("When Robots take Over was added to your cart.");
	    },
	    cta: "Buy Now"
	  },
	  {
	    title: "Looking at the Stars",
	    copy: "A tough look at why it never pays to structure you life off of what you believe a famous person must live like",
	    clickHandler: function(e){
	      console.log("Looking at the stars was added to your cart.");
	    },
	    cta: "Buy Now"	          
	  }
	];

Now I am going to be alternating between the both of them so I need to know what number I'm on.

	var currentSection = 0;
	
Next I'll specify my template. It is easiest if this is a string. You should be able to use `<script>` tags with `type="text/template` but I haven't been able to get it working. Something about [unrecognized expresions](http://stackoverflow.com/questions/14347611/jquery-client-side-template-syntax-error-unrecognized-expression). But back to how I use it, and what we know works.

	var sectionTemplate = [ '<section>',
	      '<h3>{{title}}</h3>',
	      '<p>{{copy}}</p>',
	      '<a href="#" on-click="clickHandler">{{cta}}</a>',
	   '</section>'].join('\n');

Next we make our template by combining the Template and the Content.
	
	var $rotatorTemplate = TemplateMaker(sectionTemplate, rotatorContent[0]);
	$('body').append($rotatorTemplate);
	
So let's look at updating the content. I'll use `setInterval` to fire a function every 2000 ms that increments the current sections, check if it's bigger than the number of sections we have total, and if so, set it's value  to 0.
	
	setInterval(function(){	  
	  currentSection++;
	  if(currentSection >= rotatorContent.length){
	    currentSection = 0;
	  }
	  	  
	  $rotatorTemplate.update(rotatorContent[currentSection]);	
  
	}, 2000);

It's really that easy. Send it new data to update with, and It will update with it.

[JSBin Example](http://jsbin.com/rokini/27/edit)

Limitations
---
This isn't a mustache replacement. It only works for the content between tags. And elements that implement the `on-click` data attribute. Expanding this library will be a side project. But do feel free to send me a pull request to  features you've added to your clone.

If you don't know what you're doing, I'm sure you can break it. Be ready to troubleshoot while you add features. It's writen logically, and those who just know a little jQuery can figure it out with some Googling.


Wishlist
--
hook up inview to call a "inview" property when the template shows up on the screen - https://github.com/protonet/jquery.inview
