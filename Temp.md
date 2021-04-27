### Overview & Principle

#### Reactive Native(RN)：
React Native is an open-source cross-platform mobile application UI framework created by Facebook. It was first released in 2015. It mainly contains of 3 parts React(JS),Native(Oc/Java), [JSI(JavaScript Interface)](https://formidable.com/blog/2019/jsi-jsc-part-2/) or Bridge and Js engine(C++). Js code interpreted by Js engine and build its virtual DOM tree,then it control the native view tree(similar real dom tree in browser) throught JSI or bridge. There are still more details in the whole process(eg:diff algorithm optimization and yoga layout conversion), we will not go into details here.We will focus on the JSI/Bridge and Js engine.
<br/>We can see the principle from the simple process:

- The process is longer than the original, so the principle determines the UI performance of RN can never exceed the native.
- It do not have its own whole UI system. It cann't reduce any of native issues, but will increase the const of solving all kinds of native issues. Because it handles the native UI detail process by itself,we are difficulty to hook it. Some non-UI issues we must fix it on native side and interact with JS side by bridge.
- These additional processes must go throught JS-->C++-->ObjectC/Java. The architecture has been optimized by JSI, but it only optimized the process of JS-->C++.The C++-->ObjectC/Java is still an unsolvable performance bottleneck on android side.Since it must via JNI,this will cause serialize and deserialize, and it is asynchronous.But this will not be a problem on iOS side, since Oc/Swift call C++ directly without any additional loss. This is one of the reasons why RN performs better on iOS than on Android.
- There are some other performance issues such as high memory usage, low start-up speed, app package size. Facebook found it mainly cause by Js engine. So they officially released a new generation of lightweight JavaScript engine [Hermes](https://hermesengine.dev/) at the ChainReact2019. It can be optionally used at least version 0.60.4 of React Native. *I discuss it with @Jonathan Guo,he indicate that Wonder App use 0.62.2 of React Native,but disable the Hermes.Since it is unstable.* 


#### Flutter
Flutter is an open-source UI software development kit created by Google. It was first released in 2017. It refers to the excellent design ideas of RN,eg:virtual DOM,Data binding and driven,Componentization. 
We can see the principle from the simple process:
- It use its independent measure,layout and render engine,so its UI performance is likely to be comparable to or even surpassing native.So it was designed as the UI framwork of [Fuchsia](https://zh.wikipedia.org/wiki/Google_Fuchsia).


#### Kotlin Multiplatform Moblie(KMM)
KMM is built on top of the [Kotlin Multiplatform](https://kotlinlang.org/docs/mpp-intro.html) technology. KMM is different from RN and Flutter,it is not a UI framwork,it is more like the magic of the compi.

### Basic situation

#### Stability
We can see the stability order Flutter > RN > KMM. However,KMM is in [Alpha](https://kotlinlang.org/docs/components-stability.html),Kotlin team is fully committed to working to improve and evolve this technology and will not suddenly drop it.The update speed of RN is really slow. From 2015 to now, it is still 0.64 version. Hermes released in 2019 is still unstable. When we meet its architectural problems, we will be difficult to quickly fix.

#### Use Status
k'

#### Simple Performance comparison
2g.

#### Personnel cost
41

### Additional issues with hybrid solutions
3

### Future:
#### da:
ad


