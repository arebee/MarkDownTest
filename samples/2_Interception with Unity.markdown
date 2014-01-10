---
Source File Name: 75-Interception.docx
AssetID: 8354c203-e77a-4873-80c9-b9041b468795
Title: Interception with Unity
Order In ToC: 2
Output Filename: 2_Interception with Unity.markdown
---

#### Markdown Test ####
# Interception with Unity #
----------


&gt; ![](images/note.gif)#!155CharTopicSummary!#:
&gt; 
Unity provides instance and type interception, enables interception with either objects resolved through the container, or by the stand-alone API.
Unity interception enables you to effectively capture calls to objects and add additional functionality to the target object. Interception is useful when you want to modify the behavior for individual objects but not the entire class, very much as you would do when using the [Decorator pattern](http://en.wikipedia.org/wiki/Decorator_pattern). It provides a flexible approach for adding new behaviors to an object at run time.   
This section contains the following topics that will help you to understand interception:  
+ <a href="#interception_about_unity" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">About Unity Interception</a>. This section of this topic describes the basic principles of interception in Unity.
+ [Scenarios for Interception](test-markdown_ed7289ac-2407-409a-8a4d-04bc2a0c763a.html). This topic describes common scenarios addressed by Unity interception.
+ [Behaviors for Interception](test-markdown_73690eea-cce2-48aa-a143-d6fcd80e1654.html). This topic describes behaviors you might implement with the **IInterceptionBehavior** interface to configure a container through the **RegisterType** method.
+ [Configuring a Container for Interception](test-markdown_6a974ef0-4f5e-407f-b196-b126a08f9205.html). This topic describes how to configure the Unity container for interception.
+ [Unity Interception Techniques](test-markdown_9765b670-328a-488c-a219-d114381b7c75.html). This topic describes in detail the design of Unity interception.
+ [Using Interception in Applications](test-markdown_67c6df4d-4c1b-4020-82c8-f2cf87defbc2.html). This topic describes how you use Unity interception in your applications.
+ [Using Interception and Policy Injection](test-markdown_7a2c7fa6-28c2-479e-8df9-b4651824eb94.html). This topic explains how policy injection through interception works in Unity, how you can use matching rules to select target classes and class members for policy injection, and how you can use the Enterprise Library call handlers with Unity.

&gt; ![](images/note.gif)Note:
&gt; You cannot use Unity Interception in Windows Store apps. For more information, see [Target Audience and System Requirements](test-markdown_c678b828-6309-41e9-bc24-04c290d448bb.html).

# About Unity Interception #
<a name="interception_about_unity" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Unity provides both instance and type interception, and enables interception with either objects you resolve through the container, or by using the stand-alone API to explicitly invoke interception on a known instance.  


&gt; ![](images/note.gif)Note:
&gt; Unity interception can be used without a Unity dependency injection (DI) container by using the stand-alone API through the static **Intercept** class. For more information see [Using Interception in Applications](test-markdown_67c6df4d-4c1b-4020-82c8-f2cf87defbc2.html).

The interception mechanism captures the call to an object at invocation time and provides the full implementation of the object. Unity uses the **Interceptor** class to specify the interception mechanism to be used, how interception happens, and the **InterceptionBehavior** class to describe what to do when an object is intercepted. Unity interception is designed to have its behaviors performed on the entire object and all of its methods. Interceptors play a role only at proxy (or derived type) creation time. Once the proxy, or derived type, is created, the interceptor has provided any required components for the intercepted object and incorporated them into the proxy's processing. The proxy can then complete its processing with no further input from the interceptor.  


&gt; ![](images/note.gif)Note:
&gt; Proxy or type-creation time is the point of the largest performance hit.

Unity interception uses a single behaviors pipeline encompassing all of the behaviors for processing interception. The pipeline consists of an essentially fixed series of elements, the behaviors that you have set up, that perform pre- and/or post-processing logic on the object. If so dictated by the behavior element logic, each behavior element in the pipeline can end the invocations, and subsequent pipeline elements will not execute. The last executed element in the pipeline exits the pipeline and invokes the implementation of the intercepted object, thus enabling run-time processing. In post processing the process is reversed.  
![](images/68DE2665B2D4503428107A4DD26F42E3.png)  
The following table describes the basic components of the Unity interception mechanism.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Interception Component</p></th><th><p>Description</p></th></tr><tr><td><p>Interceptor</p></td><td><p>Represents an interception mechanism. The interception mechanism actually intercepts the object call. There are two kinds of interceptors, type and instance. Unity provides a single type interceptor, <b>VirtualMethodInterceptor</b>, and two instance interceptors, <b>Interface</b> and <b>TransparentProxy</b>. For more information on type and instance interceptors see <hlink xlink:type="simple" xlink:show="new" xlink:actuate="onRequest" xlink:href="9765b670-328a-488c-a219-d114381b7c75.html">Unity Interception Techniques</hlink>.</p></td></tr><tr><td><p>Interception behavior</p></td><td><p>Represents what to do when a method is intercepted. Behaviors always apply to every method on an intercepted type. Policy injection is just one such behavior.</p></td></tr><tr><td><p>Behaviors pipeline</p></td><td><p>A pipeline of behaviors that processing is routed to for interception.</p></td></tr></table>

&gt; ![](images/note.gif)Note:
&gt; Interception behaviors are a generalization of the default policy injection call handlers. The **ICallHandler** interface is very similar to the **IInterceptionBehavior** interface. A major difference is that the call handlers are passed to the method that they are called on.
The default policy injection call handlers target specific methods; but depending on the matching rules, they might target each and every method on an intercepted type, whereas Unity interception is designed to have its behaviors performed on the entire object and all of its methods. 


