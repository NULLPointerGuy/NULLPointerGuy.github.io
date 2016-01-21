---
layout: post
title: FAB to dialog transistion and vice versa.
---

**FAB Animations Part 2:**<br/>


![ScreenShot](/img/Blog/fab2.gif)


In this blog post we are gonna demonstrate transition between two activities,transistion was introduced in material design and as per the doc the definiation goes like this **Activity transitions in material design apps provide visual connections between different states through motion and transformations between common elements**.since we are only interested in shared transistion animations,lets focus on that for the moment.According to the developer website **shared transistion animation decides on how the views are shared during transistion**,including the following 4

**1.changeBounds** : change the layout border target view. <br/>
**2.changeClipBounds** : border cropped target view. <br/>
**3.changeTransform** : change the zoom target view scale, and rotation angle; changeImageTransform. <br/>
**4.changeImageTransform**: change target picture size and scaling.<br/>

shared elements should have one additional attribute  **android:transitionName = ""**

<!--break-->

so before we dive into animation,let's have a brief description about the classes used in the animation.

>**1.MorphFabToDialog.java**:A custom transition that morphs a circle into a rectangle, changing it's background color and extends changeBounds transition.<br/>
while defining custom transitons there are three methods we need to override **capture starting values**,**capturing end values** and **createAnimator**.<br/>
super.captureStartValues(transitionValues) && super.captureEndValues(transitionValues) the framework calls this method for every view in the starting scene and ending scene respectively, after this we should be adding custom values to transistionValues.<br/>
We can get reference of the view that is transistioning using **transitionValues.view** and also it contains Map instance in which you can store the view values(as said before) you want, in this example **rectMorph:color** and **rectMorph:cornerRadius** are the two map keys for which we enter values as accent color and 4dp respectively.<br/>
And the third method that is createAnimator return animator for animating between captureStartValues and captureEndValues. Since we animate both color and corner radius we will be returning animator set(which is derived from animator),also we ease in childview of the view being animated.<br/>

**Note**: Here the animatorset uses  playTogether method so that animations are performed simultanously.<br/>
For more info about the custom transistions visit the official [developer site](http://developer.android.com/training/transitions/custom-transitions.html).

>**2.MorphDialogToFab.java**:A custom transition that morphs a rectangle into a circle and functionality is exactly oposite of the above explained transition.

>**3.MorphDrawable.java**: In both the transitions we use a custom drawables called MorphDrawable, whose functionality is to morph size, shape (via it's corner radius) and color given them as a paramters. This class extends Drawable, basically set the corner alpha and the color in the canvas and draws them during animation.

For more about drawable check the official [developer site](http://developer.android.com/reference/android/graphics/drawable/Drawable.html).

Having described the classes lets check the code.

>Intent intent = new Intent(GoalDashboard.this, CreateGoal.class);<br/>
 ActivityOptions options = ActivityOptions.makeSceneTransitionAnimation(GoalDashboard.this, fab, getString(R.string.fabtransition));<br/>
 startActivity(intent ,options.toBundle());

before starting the activity here we defined shared view, also remember to use the same string as that of android:transitionName of fab button.

And in the reciving activity 

>ArcMotion arcMotion = new ArcMotion();
 arcMotion.setMinimumHorizontalAngle(50f);
 arcMotion.setMinimumVerticalAngle(50f);

set the motion path along with the horizontal and vertical angle's.


> Interpolator easeInOut = AnimationUtils.loadInterpolator(this, android.R.interpolator.fast_out_slow_in);

create instance of ease in interpolator and set it in both shared enter transistion and exit transistion, before that MorphFabToDialog and MorphDialogToFab instances and set path as arc motion, don't forget to add setSharedElementEnterTransition() and setSharedElementReturnTransition() for the getWindow() object.

>MorphFabToDialog sharedEnter = new MorphFabToDialog();<br/>
 sharedEnter.setPathMotion(arcMotion);<br/>
 sharedEnter.setInterpolator(easeInOut);<br/>
 MorphDialogToFab sharedReturn = new MorphDialogToFab();<br/>
 sharedReturn.setPathMotion(arcMotion);<br/>
 sharedReturn.setInterpolator(easeInOut);<br/>
 Window window = getWindow();<br/>
 window.setSharedElementEnterTransition(sharedEnter);<br/>
 window.setSharedElementReturnTransition(sharedReturn);<br/>


 To find out more in detail check this blog [post](http://hujiaweibujidao.github.io/blog/2015/12/13/Fab-and-Dialog-Morphing-Animation/)<br/>






