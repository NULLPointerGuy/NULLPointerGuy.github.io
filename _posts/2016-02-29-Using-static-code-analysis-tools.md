---
layout: post
title: Using static code analysis tools.
---
**1.Facebook Infer:**

Facebook Infer for android reports potential **null pointer exceptions and resource leaks** in Android and Java code.<br/>
Checkout the get started page to setup [infer](http://fbinfer.com/docs/getting-started.html) environment.<br/>

**Note:** Please make sure you have installed all the dependencies required.<br/> ./build-infer.sh  script should return with no issues found therefore compiling the infer.

Once the infer is compiled and ready, download the gradle wrapper by running ./gradlew clean, now use infer  ---- ./gradlew build.

>But i personally suggest usage of  **infer ----debug ---- ./gradlew build** that outputs the additional debug informations such as possible Class Cast Exception etc and generates html pages for each file about analysis done.

Also in the build.gradle add lintOptions so that it will stop gradle build in case of any errors found.

{% highlight groovy %}

android {
	lintOptions{
		abortOnError false
	}
}

{% endhighlight %}

>Please note that if your running infer multiple times either use gradle clean or use infer -- incremental ./gradlew build.

<!--break-->


>**TroubleShooting**: If you get error saying infer command not found re-compile the infer using ./build-infer.sh and then try again, in case the error still persists then infer is not compiled properly, this might be due to the reason that all dependencies are not installed in your computer. 

After a successful Infer run,infer-out a directory is created to store the results of the analysis.<br/>

![ScreenShot](/img/Blog/infer.png) 

**Structure of infer-out:**

**1.Procs.csv and stats.json:** contain debug information about the run.<br/>
**2.Bugs.txt, report.csv, and report.json:** contain the Infer reports in three different formats.<br/>
**3.The log/, multicore/, and sources/ folders:** are used internally to drive the analyzer.<br/>
**4.Captured:** contains information for each file analyzed by Infer(also see the html pages with detailed analysis if infer is used with **debug** option).<br/>

Checkout my [sample app](https://github.com/callmekarthik/Playground-App) that has infer-out folder along with the html pages with detailed analysis inside captured folder.

**2.Android Lint:**
Code analysis tool that checks your Android project source files for potential bugs and optimization improvements for correctness.<br/>

This is inbuilt tool in Android Studio, and it automatically runs once you compile the code,you can also run it manually by right clicking in the **IDE Analyze > Inspect Code**, moreover you can also analyze dependencies, cyclic dependencies etc.<br/>
You can also invoke this through command line<br/>

**$ ./gradlew lint** (in linux and osx)<br/>

And the Lint results will be a html page in the **mobile/build/outputs/lint-results.html** i personally feel lint in a way is more advanced and consider's more paramters into the account while dumping the results in the  **lint-results.html**.<br/>

![ScreenShot](/img/Blog/lint.png) 

The optimization tips in the lint-results.html will take all the paramters(such as Security,Performance,Overdraw,Unused resources etc) into the consideration.<br/>For more details about the [Lint](http://developer.android.com/tools/help/lint.html) visit the developer website.<br/>

**Further Reading:**<br/>
You can also build Custom Lint checks if you think that built-in Lint checks are not enough.You can get started with **Custom Lints** by visiting this [website](https://lab.getbase.com/custom-lint-checks-part-1/)<br/>





