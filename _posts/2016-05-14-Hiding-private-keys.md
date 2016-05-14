---
layout: post
title: Hiding API keys in java and xml.
---

Now let's assume you have an android application that uses third party services such as google's places API and mapbox sdk.Given those two services work with help of API key, it is users reponsiblity to not to check in such sensitive information into the version control systems.

**Scenario 1:**<br/>
You have a public static final string declared in the class which hold the API key value, now in such cases make sure you have gradle.properties file in your project.
If no manually create the file and **add gradle.properties to the gitignore** now you can add APIKEY as key as shown below inside gradle.properties

   {% highlight xml %}
    APIKEY = "abcdefgh"
   {% endhighlight %}

now you need to inform your build.gradle to create a field in config called APIKEY(add it in the defaultConfig of android in build.gradle) which is done in the following way

  {% highlight xml %}
    buildConfigField("String", "API_KEY", APIKEY)
  {% endhighlight %}

<!--break-->

**Note:**<br/>
1.Since i have declared as APIKEY in gradle.properties make sure use the same while adding the above line in the build.gradle.<br/>
2.Now you can access the buildconfig api key as BuildConfig.API_KEY in your java files, this is because of the sole reason that i have added **"API_KEY"** in the build config statement.<br/>
3.Please make sure that you have added gradle.properties to gitignore file.

**Scenario 2:**<br/>
In this case let's assume that you are using mapbox sdk, were you have define the key in the xml.Now in such cases and you don't want to store it in the strings.xml or directly use it in the xml.<br/>
As mentioned above please create gradle.properties(if does not exists) and add it to the gitignore, define mapbox api in gradle.properties exactly as done in the above example.
Now the only thing that changes is how you inform the build.gradle file, which is achived as shown below.

{% highlight xml %}
  resValue "string", "api_key", MAPBOXAPI
{% endhighlight %}

you can refer the api key string in the xml as **@string/api_key**.

**Note:**<br/>
1.Here MAPBOXAPI refers to the MAPBOXAPI declared in the gradle.properties similar to what we did in above example.<br/>
2.Also make sure that you don't have any strings by name **api_key** in the strings.xml, as it will be generated automatically. 


**Further Reading:**<br/>
1.How to Hide your API key in Android Apps by [Allan Caine](https://www.linkedin.com/pulse/how-hide-your-api-key-android-apps-allan-caine).<br/>
2.A stack overflow answer by [CommonsWare](http://stackoverflow.com/questions/28306657/how-to-get-buildconfig-values-from-xml)



