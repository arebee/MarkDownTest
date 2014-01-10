---
Source File Name: 75-Interception.docx
AssetID: 67c6df4d-4c1b-4020-82c8-f2cf87defbc2
Title: Using Interception in Applications
Order In ToC: 2\5
Output Filename: 2\5_Using Interception in Applications.markdown
---

#### Markdown Test ####
# Using Interception in Applications #
----------


&gt; ![](images/note.gif)#!155CharTopicSummary!#:
&gt; 
This topic describes how to use Unity interception in your applications.
This topic describes how to use Unity interception in your applications. You can use Unity interception with a dependency injection (DI) container or as a stand-alone feature with no DI container.   
![](images/3397ADBCC9BA37E1603B111630672796.png)  
When you use a DI container, the container provides a reference to the intercepted object. When the container resolves an object that is configured for interception, the object is intercepted as defined in the configuration. When performing stand-alone interception, you must already have a reference to the object you want intercepted. You invoke the stand-alone interception API and explicitly intercept the object. For more information on using containers, see [Dependency Injection with Unity](test-markdown_16137689-c8fb-46b0-87f5-7f975241832f.html).  
Unity interception requires an interceptor, and a collection of interception behaviors that comprise a pipeline. Interceptors are used when performing interception through the container and through the stand-alone API. Using the container results in adding interception objects that are being resolved, while using the stand-alone API enables you to perform just interception.   
In both cases you can implement additional interfaces on intercepted objects. You must provide the information for the additional interfaces that will be added for interception. Using additional interfaces has some limitations. You cannot add open generic interfaces and you cannot reimplement an interface that has already been implemented with nonvirtual methods. Additional interfaces are supported by all three types of interceptors. When you are performing interface interception, you can only invoke methods on the particular interface you used to intercept and any additional interfaces that you have specified. Casts to other interfaces implemented by the intercepted object will fail.  
Though behaviors normally can have both pre- and post-processing functionality, with any additional interfaces the behaviors must handle the processing in its entirety. There cannot be any post-processing because the original object has no implementation for the additional interfaces’ methods. Any calls to the additional interface methods handled by the behaviors will result in a **NotImplementedException** being thrown.  
You can also intercept abstract classes with abstract methods, allowing the behaviors pipeline to provide the implementation for these abstract methods. This is similar to adding interfaces in that in both cases there are undefined methods for which an implementation must be provided, but they are different mechanisms. Abstract class interception only works with the **VirtualMethodInterceptor**. You can implement abstract classes with abstract methods. If you intercept a type that already implements some interfaces, both virtual method and transparent proxy interception allow for casting to these interfaces and invoke methods on them, which will be intercepted. There cannot be any post processing because the original object has no implementation for the additional interfaces’ methods  
This topic contains the following sections:  
+ [Stand-alone Unity Interception](test-markdown_29f44713-c61a-4b7c-a21e-9cd4d183ace3.html#interception_standalone) describes how to use Unity interception as a stand-alone feature.
+ [Interception Behavior Pipeline](test-markdown_7f7a1362-150a-417c-8b15-ba4f7d08cd5a.html#interception_pipeline) describes how to add a behavior and interceptor to a new interception behaviors pipeline.
+ [Interception with a Container](test-markdown_20b2c058-da8b-43ba-b0b5-40d919915481.html#interception_container) describes interception with a container.

