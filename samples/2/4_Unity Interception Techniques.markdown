---
Source File Name: 75-Interception.docx
AssetID: 9765b670-328a-488c-a219-d114381b7c75
Title: Unity Interception Techniques
Order In ToC: 2\4
Output Filename: 2\4_Unity Interception Techniques.markdown
---

#### Markdown Test ####
# Unity Interception Techniques #
----------


&gt; ![](images/note.gif)#!155CharTopicSummary!#:
&gt; 
The two common approaches to the interception process are instance and type, and Unity provides techniques for both.
In order to perform interception, Unity must be able to capture the original call and pass it through a behaviors pipeline to the target object, then pass the result back through the behaviors pipeline to the original caller. The two common approaches to the interception process are instance interception and type interception, and Unity provides techniques for both. Instance interceptors work by creating a proxy to the intercepted instance. Type interceptors work by deriving a new type that implements interception. The instance interception target is the original, non-intercepted object, but when performing type interception the target is intercepted and a derived object used.  


&gt; ![](images/note.gif)Note:
&gt; Instance interception works only on instance methods; it does not work for static methods or constructors since the constructor has already executed by the time the client code gets back an interception-ready object. Instance interception can only intercept public instance methods. Type interception can intercept public and protected methods.

This topic contains the following sections that will help you to understand interception:  
+ <a href="#_Instance_Interception" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Instance Interception</a>
+ <a href="#_Type_Interception" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Type Interception</a>
+ <a href="#_Comparison_of_Interception" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Comparison of Interception Techniques</a>
+ <a href="#_Summary_of_Interception" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Summary of Interception Approaches</a>
<a name="_Instance_Interception" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc236798401" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Instance Interception #
<a name="interception_instance" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Instance interception works with both existing instances of objects, and with new instances created by Unity. The following schematic shows the basic process of instance interception.   
![](images/2FE4AF6648C3A3CE0101D8BBD767C0E2.png)  
When the application resolves the object through the Unity container, the Unity interception container extension manages the process. It obtains a new or existing instance of the object from the container, and creates a proxy to the object. Then it creates the handler pipeline and connects it to the target object before returning a reference to the proxy. The client then calls methods and sets properties on the proxy as though it were the target object.   

&gt; ![](images/note.gif)Note:
&gt; Unity interception can be used without a Unity DI container by using the stand-alone API through the static **Intercept** class. For more information, see [Using Interception in Applications](test-markdown_67c6df4d-4c1b-4020-82c8-f2cf87defbc2.html).
These calls pass through the interception behaviors, executing the preprocessing stage of each one, with the final behavior in the chain passing the call to the target object. The return value from the target object passes back through the behaviors in the reverse order, executing the post-processing stage of each one. The first behavior in the pipeline then passes the result back to the caller.   
Instance interception is the most common and widely used technique. It can be used with objects that either implement the **MarshalByRefObject** abstract class, or implement a public interface that defines all of the methods to be intercepted. Unity provides the two interceptors, **TransparentProxyInterceptor** and **InterfaceInterceptor,** that support these two scenarios. For more details, see the tables in <a href="#interception_comparison" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Comparison of Interception Techniques</a> later in this topic.  
<a name="_Type_Interception" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc236798402" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Type Interception #
<a name="interception_type" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Type interception uses a derived class instead of a proxy. As described in the previous section, instance interception works by creating a proxy to the target object. Type interception, on the other hand, more closely resembles aspect-oriented programming (AOP) techniques common in Java-based systems. Type interception avoids the possible performance penalties of using a proxy object by dynamically deriving a new class from the original class, and inserting calls to the behaviors that make up the pipeline. The following schematic shows the basic process of type interception.  
![](images/19801E7EC4B67B413FB471679CBF6670.png)  
When the application resolves the required type through the Unity container, the Unity interception container extension creates the new derived type and passes it, rather than the resolved type, back to the caller. Because the type passed to the caller derives from the original class, it can be used in the same way as the original class. The caller simply calls the object, and the derived class will pass the call through the behaviors in the pipeline just as is done when using instance interception.   

&gt; ![](images/note.gif)Note:
&gt; Unity interception can be used without a Unity DI container by using the stand-alone API through the static **Intercept** class. For more information, see [Using Interception in Applications](test-markdown_67c6df4d-4c1b-4020-82c8-f2cf87defbc2.html).
However, due to the nature of the dynamic type generation, there are some limitations with this approach. It can only be used to intercept public and protected virtual methods, and cannot be used with existing object instances. In general, type interception is most suited to scenarios where you create objects especially to support interception and allow for the flexibility and decoupling provided by policy injection, or when you have mappings in your container for base classes that expose virtual methods. For more details, see the tables of comparisons in the following topic <a href="#interception_comparison" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Comparison of Interception Techniques</a>.  
<a name="_Comparison_of_Interception" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc236798403" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Comparison of Interception Techniques #
<a name="interception_comparison" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The previous sections demonstrated how it is important to choose the appropriate interception technique based on your requirements and the type of object you want to intercept. The following table lists the three interceptor classes included in Unity, and describes when you should use each type.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Type</p></th><th><p>Description</p></th><th><p>Use</p></th></tr><tr><td><p><b>Transparent</b></p><p><b>Proxy</b></p><p><b>Interceptor</b></p></td><td><p>An instance interceptor. The proxy is created by using the .NET <b>TransparentProxy/RealProxy</b> infrastructure.</p></td><td><p>When the type to intercept is a <b>MarshalByRefObject</b> or when only methods from the type's implemented interfaces need to be intercepted.</p></td></tr><tr><td><p><b>Interface</b></p><p><b>Interceptor</b></p></td><td><p>An instance interceptor. It can proxy only one interface on the object. It uses dynamic code generation to create the proxy class. </p></td><td><p>When resolving an interface mapped to a type.</p></td></tr><tr><td><p><b>Virtual</b></p><p><b>Method</b></p><p><b>Interceptor</b></p></td><td><p>A type interceptor. It uses dynamic code generation to create a derived class that is instantiated instead of the original intercepted class, and to hook up the behaviors.</p></td><td><p>When only virtual methods need to be intercepted.</p></td></tr></table>
Selection of a specific interceptor depends on your specific needs, because each one has various tradeoffs. The following table summarizes the three interceptors and their advantages and disadvantages.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Type</p></th><th><p>Advantages</p></th><th><p>Disadvantages</p></th></tr><tr><td><p><b>Transparent Proxy Interceptor</b></p></td><td><p>Can intercept all methods of the target object (virtual, non-virtual, or interface). </p></td><td><p>The object must either implement an interface or inherit from <b>System.MarshalByRefObject</b>. If the marshal by reference object is not a base class, you can only proxy interface methods. The Transparent Proxy process is much slower than a regular method call.</p></td></tr><tr><td><p><b>Interface Interceptor</b></p></td><td><p>Allows interception on any object that implements the target interface. It is much faster than the <b>TransparentProxyInterceptor</b>.</p></td><td><p>It only intercepts methods on a single interface. It cannot cast a proxy back to the target object's class or to other interfaces on the target object.</p></td></tr><tr><td><p><b>Virtual Method Interceptor</b></p></td><td><p>Calls are much faster than the Transparent Proxy Interceptor.</p></td><td><p>Interception only happens on virtual methods. You must set up interception at object creation time and cannot intercept an existing object.</p></td></tr></table><a name="_Summary_of_Interception" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc236798404" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Summary of Interception Approaches #
<a name="interception_summary" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Unity provides instance and type interception with intercepted objects for which you have obtained a reference either through the container, or by using the stand-alone API to explicitly intercept a known instance. Instance interceptors use a separate proxy object between your code and your target object. Using interception, you make a call on the proxy object instead of directly calling the target object. The proxy invokes the various interception behaviors, and then it forwards the call to the target object. Different implementations of instance interceptors can have different constraints. Instance interceptors have the following characteristics:  
+ They can intercept objects created by the container. 
+ They can intercept objects not created by the container. 
+ The **TransparentProxyInterceptor** can intercept more than one interface, and marshal-by-reference objects. 
+ The **InterfaceInterceptor** can only be used to intercept a single interface. 
Type interceptors create a new type that inherits from the target type. This new type is then instantiated instead of the original type you requested. The new type overrides all the virtual methods on the original target type. Type instance interceptors have the following characteristics:  
+ Only one object is created; there is no proxy object between the caller and the new object. 
+ The new object has full type compatibility because it is derived from the target type. 
+ They are able to intercept objects only at creation, and cannot intercept existing instances. 
+ They can only intercept virtual public and protected methods.
The system that Unity uses to automatically create a derived target object, or a proxy and behaviors pipeline, is similar to the aspect-oriented programming (AOP) approach. However, Unity is not an AOP framework implementation for the following reasons:  
+ It uses interception to enable only preprocessing behaviors and post-processing behaviors.
+ It does not insert code into methods, although it can create derived classes containing policy pipelines.
+ It does not provide interception for class constructors.

