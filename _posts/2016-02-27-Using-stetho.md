---
layout: post
title: Debugging with Stetho.
---

Stetho is a debug bridge for android that enables **Chrome Developer Feature** natively for android applications.<br/>

![ScreenShot](/img/Blog/stetho-network.gif)
![ScreenShot](/img/Blog/stetho-inspect1.gif)


Intialize stetho in the onCreate method of your application class.

{% highlight java %}
Stetho.initialize(
  Stetho.newInitializerBuilder(mContext)
   .enableDumpapp(Stetho.defaultDumperPluginsProvider(mContext))
   .enableWebKitInspector(Stetho.defaultInspectorModulesProvider(mContext))
   .build());
{% endhighlight %}


Ideally the best suited HTTP library are the one's which allow you to add **network interceptors**, I would personally recommend okHttpClient3.
 
<!--break-->

{% highlight java %}
OkHttpClient client =  new OkHttpClient.Builder()
                  .addNetworkInterceptor(new StethoInterceptor())
                  .build();
{% endhighlight %}

**Side Notes:** 
>(i)If you have never used okHttpClient,enqueue callback returns the response in background thread,make sure you communicate to the main thread,i have used handler for the purpose.<br/>

>(ii)If you are using Picasso to display the images in your application make sure you use the same okhttp instance that has StethoInterceptor configured in it.(personally i would recommend using [Singleton pattern](https://en.wikipedia.org/wiki/Singleton_pattern) to get instance of okhttp)<br/>


{% highlight java %}
	picasso = new Picasso.Builder(mContext)
                .downloader(new OkHttp3Downloader(okHttpClient))
                .build();	
{% endhighlight %}
make a note that i have used **OkHttp3Downloader** since i am using okhttp3 client.  

>(iii)Do not configure okhttp in the following way
{% highlight java %}
	OkHttpClient client = new OkHttpClient();
	client.networkInterceptors().add(new StethoInterceptor())
{% endhighlight %}
in okhttp3 the networkInterceptors list is made [unmodifiableList](http://www.tutorialspoint.com/java/util/collections_unmodifiablelist.htm), therefore will throw unsupported Operation Exception<br/> 


Also if your using okHttp3 please make note that for stethointerceptor,<br/>
**com.facebook.stetho:stetho-okhttp3:1.3.0** dependency used.

The video gives the quickoverview on how stetho helps in monitoring network calls, and inspecting elements in the screens.<br/>
<iframe width="420" height="315" src="https://www.youtube.com/embed/gscgCjhRWPk" frameborder="0" allowfullscreen></iframe>

**Note:**You can additionally view the shared preferences value and the database values using the stetho which i have not included in the video.In the sample application i have created two dummy methods.<br/> **fillDummyValuesInSharedPreferences** and **fillDummyValuesInDB**  which fills value in SharedPreferences and Realm Db just select Resources tab in the inspector, for SharedPreferences check local storages and for db check on the storage, **Realm db contents will not be shown**.<br/>

![ScreenShot](/img/Blog/stethodb.png) 

>Checkout the **source code** in [Animations demo repository](https://github.com/callmekarthik/AnimationsDemo), look into the **ImagerApp** and **Adapter class** in the repo.<br/>

**For more information on stetho:**<br/> 
please checkout Donn felker's video in [caster.io](https://caster.io/episodes/episode-4-debugging-android-with-stetho/) and also checkout the sample code in the [Stetho repo.](https://github.com/facebook/stetho)<br/>

Also note that stetho also provides **custom dumpapp plugins**, which i have not covered in this blog post.<br/>



