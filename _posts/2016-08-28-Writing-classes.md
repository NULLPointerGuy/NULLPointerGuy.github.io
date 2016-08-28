---
title: Writing Classes in Android(part-1).
---
Blog post is inspired from the Steve McConnell's `working classes` chapter from the book `Code Complete 2`, this post discusses few of the principles and on how to apply those principles while writing android application.

<!--more-->
 

 >Think about abstract data types first and classes second

 Now let's take an example to elaborate this principle, let's assume you have an android application which has three different screens(either activities or fragments).

 Assuming each of the three screens require `GA Events`,  `Http client`(to make network calls), `Image loading client`(such as picasso), and an action bar to display three different screen titles.

 Now normally we would implement http client as a singleton in `Application Class`, and add routines to each of the screens to send GA events, and also make image loading client also return single instance from the `Application class`(which has common config such as place holder images, logs enabled etc). 

 But what we should be ideally be doing is that we should think of `common objects/routines` required by the three screens and make them abstract.

 For example all three screens require a common routine to send GA events,now we can define an `interface` called `GaEvents` which contains abstract method named `sendGAEvents` now each of these three screens either fragments/activites can implement and sendGAEvents method(where the implementation is different for differnt classes, based on what events you need to capture).

 **Please note:**<br/>I could have also created an abstract `base class` having abstract routine sendGAEvents, but that enforces a tight coupling each of the three screens `must` implement sendGAEvents method, Now in future you decide to remove GA events from one of the screens, that would be hard as all are derived classes of the base class.
 And if you try to be smart and by overriding the method and do nothing, there is a famous principle called [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle) cutshort which basically states that **routines defined in the base class should mean samething in the derived class**.

 For the http client you can define that in base class has a routine that takes `request` and  `callback` as an arguements.

 {% highlight java %}
   protected void makeNetworkCall(Request req,Callback callback){
     /**
      * with this approch you can add all interceptors, config's 
      * in one place and then just call makeNetworkcall in dervied class.
      * with each dervied class implementing callbacks
     **/

     //okHttpClient as instance of okhttp
     okHttpClient.newCall(request).enqueue(callback);
   }
 {% endhighlight %}

 Not only we hide the unneccessary implementation details of making network calls, but also unneccessary `repeation` of network client object in each class, which brings us to another principle called `dry`(do not repeat yourself).

 **Please Note:**<br/>I could have easily choose an interface, with abstract method  `makeNetworkCall` and made every class implement the interface, then each of my class need to have an network client object(which often results in code duplicity) it's also silly to assume that each of three class would have different implementation or different network client types, hence i `strongly coupled` it with the base class.
 Also it withholds `Liskov substitution principle` discussed above.

 Now comming to the other part, setting title in each of the screens, this also can be defined in a routine in base class which takes two parameters title and a boolean which says(should the back button be displayed).

{% highlight java %}
 protected void setTitle(String title,boolean isHomeRequired){
 	setTitle(title)
 	if(isHomeRequired){
 	  getSupportActionBar().setHomeButtonEnabled(true);
 	  getSupportActionBar().setDisplayHomeAsUpEnabled(true);
 	  getSupportActionBar().setDisplayShowHomeEnabled(true);
 	}
 }
{% endhighlight %}

Same reason holds good, as of why i added `setTitle` routine in the base class. 

More principles will be discussed in my next post, until then `keep calm and debug`.

