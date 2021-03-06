### Overview & Principle
<img src="https://raw.githubusercontent.com/peytonwupeixin/Draw-IO-Store/main/Simple%20UI%20progress.png">

#### Reactive Native(RN)
React Native is an open-source cross-platform mobile application UI framework created by Facebook. It was first released in 2015. It mainly contains of 3 parts React(JS),Native(Oc/Java), [JSI(JavaScript Interface)](https://formidable.com/blog/2019/jsi-jsc-part-2/) or Bridge and Js engine(C++). Js code interpreted by Js engine and build its virtual DOM tree,then it control the native view tree(similar real dom tree in browser) throught JSI or bridge to show the page. There are still more details in the whole process(eg:diff algorithm optimization and yoga layout conversion), we will not go into details here.We will focus on the JSI/Bridge and Js engine.
<br/>We can see the principle from the simple process:

- **Issues more and harder to fix than native.** It didn't have its whole UI system, it just use the origial native view to show its page. So it will be have all kinds of native system issues，eg:system versionissues,screen size issues and so on. It control the native code by itself and we are difficulty to fix them unless we customize the entire RN architecture.
- **UI performance poorer than native.** The process is longer than the native, so the principle determines the UI performance of RN can never exceed the native.Its additional processes must go throught JS-->C++-->ObjectC/Java. The architecture has been optimized by JSI, but it only optimized the process of JS-->C++.The C++-->ObjectC/Java is still an unsolvable performance bottleneck on android side.Since it must via JNI,this will cause serialize and deserialize, and it is asynchronous.But this will not be a problem on iOS side, since Oc/Swift call C++ directly without any additional loss. This is one of the reasons why RN performs better on iOS than on Android.
- **Other performance issues.** There are some other performance issues such as high memory usage, low start-up speed, app package size. Facebook found it mainly cause by Js engine. So they officially released a new generation of lightweight JavaScript engine [Hermes](https://hermesengine.dev/) at the ChainReact2019. It can be optionally used at least version 0.60.4 of React Native. *I discuss it with @Jonathan Guo,he indicate that Wonder App use 0.62.2 of React Native,but disable the Hermes.Since it is unstable.* 


#### Flutter
Flutter is an open-source UI software development kit created by Google. It was first released in 2017. It refers to the excellent design ideas of RN,eg:virtual DOM,Data binding and driven,Componentization. 
We can see the principle from the simple process:
- **Not native UI issues.** It use its independent measure,layout and render engine,so it would not have any native UI issues.Its UI performance is likely to be comparable to or even surpassing native.So it was designed as the UI framwork of [Fuchsia](https://zh.wikipedia.org/wiki/Google_Fuchsia).
- **Compatibility issues on iOS.** Currenty, the ui performance is not good as native on iOS.

#### Native
Native is to use SDK of mobile system for development. It have the best perform, but requires iOS and Android development to implement separate logic and UI on corresponding end. 

<img src="https://raw.githubusercontent.com/peytonwupeixin/Draw-IO-Store/main/Simple%20build%20and%20package.png" width="3540" height="1600">

#### Kotlin Multiplatform Moblie(KMM)
KMM is built on top of the [Kotlin Multiplatform](https://kotlinlang.org/docs/mpp-intro.html) technology. KMM is different from RN and Flutter,it is not a UI framwork,it is more like the magic of the compilation stage. It is more focus on the common business logic, and this shared common Kotlin code is compiled to different output formats for different targets: to Java bytecode for Android and to native binaries for iOS. It is only different from native app in terms of project structure and build process. App package and runtime phase is the same as native. It use native UI framework , since platform-specific UI have best performance. We can customize specific native features with the expect/actual pattern to seamlessly write platform-specific code.
<img src="https://raw.githubusercontent.com/peytonwupeixin/Draw-IO-Store/main/confluence/KMM_1.png">
<img src="https://raw.githubusercontent.com/peytonwupeixin/Draw-IO-Store/main/confluence/KMM_2.png">
<img src="https://raw.githubusercontent.com/peytonwupeixin/Draw-IO-Store/main/confluence/Kmm_3.png">
We can decide how much business code is in the shared module according to our actual situation，it is seamless.



### Basic situation

  |  Framework |Latest stable version|Language and technology stack|Main develop IDE|Who use|
  |----|---|---|---|---|
  |[RN](https://github.com/facebook/react-native) |0.64.0|JSX,Js/Ts,Redux/Vue,npm,React Native|VS Code，WebStorm or other front-end supported IDE and Xcode(iOS)|https://reactnative.dev/showcase|
  |[Flutter](https://github.com/flutter/flutter) |2.0.5|Dart,Flutter|Android Studio and Xcode(iOS)|https://flutter.dev/showcase| 
  |Native |Same as the latest system version|Kotlin or Java / Swift or Oc|Android Studio / Xcode(iOS)|Most apps| 
  |[KMM](https://github.com/jetbrains/kotlin) |0.2.3(stability level of KMM is Alpha)|Kotlin |Android Studio and XCode(iOS)|https://kotlinlang.org/lp/mobile/case-studies|

#### Stability
We can see the stability order Native>Flutter > RN > KMM. 
Flutter perform not so well as native on iOS. However,KMM is in [Alpha](https://kotlinlang.org/docs/components-stability.html),Kotlin team is fully committed to working to improve and evolve this technology and will not suddenly drop it.The update speed of RN is really slow. From 2015 to now, it is still 0.64 version. Hermes released in 2019 is still unstable. When we meet its architectural problems, we will be difficult to quickly fix. 

#### Use Status
  - [Airbnb sunsett RN after a series of uses in 2018](https://medium.com/airbnb-engineering/sunsetting-react-native-1868ba28e30a)
  - I use [LibChecker](https://play.google.com/store/apps/details?id=com.absinthe.libchecker&hl=zh&gl=US) to do a simple analysis for some Apk.I analyzed the [Google Play Users’ Choice Awards 2020](https://play.google.com/store/apps/editorial_collection/promotion_topic_bestof2020_uv_hub) and many other similar products (Uber Eats:Food Delivery,Fleet Postmates,DeliveryPanada,GH Drivers,EASI Driver).I found none of them use RN or Flutter.
**The final apk of KMM is the same with native apk,so we can’t simply analyze whether they use the KMM**

I noticed that Uber Eats has a total of 3 related applications as below:
  - [Uber Eats:Food Delivery](https://play.google.com/store/apps/details?id=com.ubercab.eats&hl=en_US&gl=US)
  - [Uber Eats Orders](https://play.google.com/store/apps/details?id=com.uber.restaurants&hl=en_US&gl=US)
  - [Uber Eats Manager](https://play.google.com/store/apps/details?id=com.uber.restaurantmanager&hl=en_US&gl=US)<br/>
  I found that the Orders app and Manager App both use RN, but the Food Delivery App didn't use RN. This may give us some inspiration.
<img src="https://raw.githubusercontent.com/peytonwupeixin/Draw-IO-Store/main/confluence/uber_eats_orders.jpg" height="640">
<img src="https://raw.githubusercontent.com/peytonwupeixin/Draw-IO-Store/main/confluence/uber_eats_manager.jpg" height="640">
<img src="https://raw.githubusercontent.com/peytonwupeixin/Draw-IO-Store/main/confluence/uber_eats_food_delivery.jpg" height="640">

#### Simple Performance comparison
We use a same table to stay at the simple middle of our 4 kinds solution demo(we can download from [DIRVER-859](https://wonder.atlassian.net/browse/DRIVER-859) and [DIRVER-861](https://wonder.atlassian.net/browse/DRIVER-861)，and take the average of 10 times.

  |Solution|Open page spend|Memory|CPU cost|
  |----|----|---|---|
  |Flutter+Native demo|117ms|246MB|1.3%|
  |Rn+Native demo|365ms|226MB|6%|
  |KMM demo |90ms|217MB|1.1%|
  |Native demo|92ms|197MB|1.3%|

#### Personnel cost
RN is friendly to front-end, they can get started quickly. But for native developer, it takes a lot of time to get familiar with the front-end ecology. The others solutions are more familiar to native developers. KMM is the closest to native, it only takes very little time to learn some differences in the early stage. Based on the time I took to develop the demo, I will assume that the time for us to develop a same page required is 1d by native each side. I will estimate the time required for us to learn other solutions and implement the same function.

  |Solution|Time|	
  |----|----|
  |Flutter+Native |2.1m/d ~ 1.6m/d |
  |Rn+Native |3.5m/d ~ 2.5 m/d|
  |KMM  |1.1m/d ~ 1.0m/d for android/kmm  + 0.5m/d for iOS = 1.6~1.5 m/d|
  |Native |1m/d for android + 1 m/d for iOS = 2m/d|

### Additional issues with hybrid solutions
- Mapbox do not have Flutter or RN SDK,so we must use a hybrid solution if use RN or Flutter,and we will meet some hybrid-special issues.
  - Data synchronization issue. The bridge between Native and RN/Flutter is asynchronous and ineffective.So the frequent data interaction(eg.polling data) between two sides may have synchronization issue.
  - Page stack issue. RN/Flutter only use one [activity](https://developer.android.com/reference/android/app/Activity)/[fragment](https://developer.android.com/reference/android/app/Fragment) with one root view(ReactRootView/FlutterView),and switch their page by internal logic in this root view. But on native side,one page is one activity. We must maintain our own hybrid page stack ourselves,when user back and forth between native page and RN/Flutter page.
  - Tool and Debug issue. There is no convenient tool to test the performance of RN part. We will develop RN in front end IDE，and native in AndroidStudio or Xcode. It is difficult to debug 2 parts of code at the same time. 

*KMM have not above issue*

### Future:
[Jetpackage Compose](https://developer.android.com/jetpack/compose) is a new UI framwork announced by Google. It release beta version recently, and I also have experienced it in my [personal demo](). It is develoed by Koltin and its own layout and measure mechanism, and use canvas on native framwork layer to draw the UI. It didn't support cross-platform currently ,so we didn't consider it this time.But his architecture design supports expansion into cross-platform in future,may be we can use it as the UI layer and KMM as the logic layer to develop our app, when it support cross-platform.


