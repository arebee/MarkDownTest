---
Source File Name: 75-Interception.docx
AssetID: 7a2c7fa6-28c2-479e-8df9-b4651824eb94
Title: Using Interception and Policy Injection
Order In ToC: 2\6
Output Filename: 2\6_Using Interception and Policy Injection.markdown
---

#### Markdown Test ####
# Using Interception and Policy Injection #
----------


&gt; ![](/images/note.gif)#!155CharTopicSummary!#:
&gt; 
Unity enables you to specify and customize any interception behavior and also enables you to use interception with or without a container.
Policy injection by a combination of Unity and the [patterns &amp; practices Enterprise Library](http://msdn.microsoft.com/entlib/) uses a set of call handlers and the equivalent call handler attributes in conjunction with the underlying Unity interception mechanism. Interception enables you to effectively capture calls to objects and provide additional functionality to the target object by using behaviors and call handlers in the pipeline to define and manage the results of the interception. In Enterprise Library, policy injection is just one implementation of a Unity interception behavior. The **PolicyInjectionBehavior** captures calls to objects you resolve through the container, and applies a policy that uses call handlers and matching rules inherited from Unity to define its policy injection behavior on a per-method basis.   
Typically, you will use this technique to change the behavior of existing objects, or to implement the management of crosscutting concerns through reusable handlers. You can specify how to match the target object using a wide range of matching rules, and construct a behavior which is effectively a policy pipeline that contains one or more call handlers.   
Calls to the intercepted methods or properties of the target object are passed through the call handlers in the order in which you add them to the pipeline, and returned through them in the reverse order. Your call handlers can access the values in the call, change these values, and control execution of the call. For example, the call handlers might authorize users, validate parameter values, cache the return value, and, if the logic so dictates, shortcut execution so that the target method does not actually execute.   
Unity enables you to specify and customize any interception behavior and also enables you to use interception with or without a container. The earlier approach to policy injection is still supported, but you can also provide policy injection by using interception behaviors.   
For information on using behaviors see [Behaviors for Interception](test-markdown_73690eea-cce2-48aa-a143-d6fcd80e1654.html).  
For information on using interception without a container see the "Stand-Alone Unity Interception" section in the [Using Interception in Applications](test-markdown_67c6df4d-4c1b-4020-82c8-f2cf87defbc2.html) topic.   
This topic contains the following sections that describe using policy injection and containers with interception:  
+ <a href="#interception_policiesinjection_flow" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Process Flow for Interception Using Policy Injection</a>
+ <a href="#interception_built_in_inj" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Using the Built-In Policy Injection Behavior</a>
+ <a href="#interception_policies" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Interception Policies</a>
+ <a href="#interception_matchingrules" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Matching Rules</a>
+ <a href="#interception_callhandlers" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Call Handlers and Policy Injection</a>
+ <a href="#interception_moreinfo" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">More Information</a>

# Process Flow for Interception Using Policy Injection #
<a name="interception_policiesinjection_flow" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>This topic describes the flow of the interception process when using policy injection in your applications.   
Policy injection processes calls to objects on a per-method basis and provides additional functionality to the target object by using behaviors in the behaviors pipeline to define and manage the results of the interception. Policies are applied on a per-method basis and use call handlers and matching rules to define the policy injection behavior. As with all interception behaviors, you can use Unity policy injection interception behavior with a dependency injection (DI) container or as a stand-alone feature with no DI container. There are two basic phases to the policy injection process, flow initialization and execution time.  
At initialization time, when the intercepted object is created or an existing object is wrapped for each interceptable method, policy injection has the following process flow:  
1. Determine which of the available policies apply to the method being intercepted. This is based on the matching rules for each policy.
2. Create a handlers pipeline for the method by combining the handlers from each of the matching policies. The order of handlers in this merged pipeline is determined by the order in which the policies were specified, plus any order override specified for individual handlers.
3. Add an entry to the method's handler pipeline mapping.
At execution time, when a method in an intercepted object is invoked, policy injection has the following process flow:   
1. The handler pipeline built for the invoked method during initialization is retrieved from the method's handler pipeline mapping. 
2. The method's handler pipeline is invoked, which effectively inserts it into the behaviors pipeline. 
&gt; ![](/images/note.gif)Note:
&gt; Calls to other methods in the intercepted object result in the handlers pipeline for each of those objects also being inserted into the behaviors pipeline.



# Using the Built-In Policy Injection Behavior #
<a name="interception_built_in_inj" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>To use the built-in policy injection behavior, configure one or more objects for interception by using **RegisterType **and specify the **PolicyInjectionBehavior** with no parameters. The following extract shows how to create the instance to include in the call to **RegisterType.**  

```csharp
new InterceptionBehavior&lt;PolicyInjectionBehavior&gt;()
```


```vb
New InterceptionBehavior(Of PolicyInjectionBehavior)()
```

The result of this is that the behavior object is resolved through the container with the container effectively supplying the constructor parameters.   
The built-in policy injection behavior searches the container for all policies, and applies only those that match and are executed when a method is called or a property of the target object is accessed. The appropriate call handler is then accessed. It will look for call handler attributes on the target class and apply any specified call handlers. The **AttributeDrivenPolicy** policy uses call handler attributes to determine which call handlers to use. Other policies are the result of setting them up with matching rules and handlers.   

&gt; ![](/images/note.gif)Note:
&gt; You do not link a policy with a target type. Instead use matching rules in each policy to select the type and members to which it will apply.
The interception mechanism's determination of which methods can be intercepted is based on which interceptor you are using, such that only virtual methods get intercepted with the **VirtualMethodInterceptor** and only interface methods get intercepted by an **InterfaceInterceptor**.  

# Interception Policies #
<a name="interception_policies" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The combination of matching rules that select target classes and methods with a pipeline containing one or more call handlers provides the functionality to define interception policies.  A policy is simply a set of call handlers that are assembled in a specific order and injected between the caller and the target object.  Policies can be specified in the configuration of the application at design time (such as in the configuration file), configured at run time using code, or specified by attributes applied to classes and members of classes.   
A rule-driven policy is one defined by either design-time or run-time configuration, and can be changed by updating the configuration. This provides a flexible approach and allows for changes to the behavior without requiring changes to the code itself.  
An-attribute driven policy is defined by applying attributes that specify individual call handlers directly to classes and class members. This means that changes to the policy will require changes to the original code, and subsequent redeployment. However, it provides a more permanent approach to policy injection that cannot be changed by users or administrators at run time.    

# Matching Rules #
<a name="interception_matchingrules" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>In order to specify which objects and which members (methods and properties) of these objects you want to intercept, you use matching rules. Matching rules are only used to determine which policies apply for each interceptable method. Unity includes a wide range of matching rules that you can use to identify objects in different ways. For example, you can specify the class name, the method or property name, the assembly name, the namespace, and more. You can even use wildcards for partial matches, and combine more than one matching rule in each policy definition. For details of the matching rules included with Unity, see [Policy Injection Matching Rules](test-markdown_412f3261-e0f5-4998-8373-5dc2ebda16af.html).   
If none of the matching rules is suitable for your requirements, you can create your own and use it with Unity. In addition, instead of using matching rules to apply policies, you can use call handler attributes to apply individual handlers to classes and their members. One special attribute can also be used to prevent policies from being applied to a class or to specific members of a class.  

# Call Handlers and Policy Injection #
<a name="interception_callhandlers" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Design patterns such as Interception, Decorator, Chain of Responsibility, and Intercepting Filter define how you can create a graph of objects, and pass a call through the series of objects. The call passes through a series of objects, often referred to as call handlers, which can access the parameters passed to a method, or the value of a property. A common use of interception is to create a handler pipeline in order to apply a predetermined policy to objects that do not implement rules or tasks that are part of the policy. For example, you may want to validate method parameters and cache the return value to maximize performance for subsequent calls to the method. You may also want to log details of the call and authorize the caller before accessing the target method.  
Instead of changing the target object to implement these functions, or writing a wrapper around the target object, you can use policy injection to inject a series of handlers into the execution pipeline based on configuration information or startup code in your application. This means that you do not have to rewrite, recompile, retest, and redeploy the original class. It also means that you can apply policies to objects for which you do not have the source code, or which you cannot modify.  
Effectively, the call from the originating class is intercepted and passed through the handlers in the chain, and then on to the original target object. Every handler can access details of the call, such as the method or property name, the parameter types and values, the returned type and value (because the call returns back through the chain in the opposite direction), and other information. Depending on the pattern and your individual requirements, objects in the chain may be able to modify the parameter or return values, block the call by not passing it to the next object in the chain, or raise an exception and pass this back through the chain to the original caller.  
Unity does not contain any implementations of call handlers. You will usually create these to implement the specific crosscutting concerns or functionality you require. However, Enterprise Library contains a series of prebuilt call handlers that you can use with Unity interception. These handlers use the other blocks in Enterprise Library to implement common crosscutting concerns such as logging, caching, exception handling, authorization, and validation. For information, see [Enterprise Library Call Handlers](test-markdown_969b6f02-4da3-41d1-8527-c9e0009d1632.html).  


&gt; ![](/images/note.gif)Note:
&gt; In Unity, a behaviors pipeline is used instead of a call handler pipeline. The behaviors pipeline can have behaviors added to it and one of these can be a policy injection behavior. You can still use code from earlier versions of Unity policy injection and Unity's underlying code will package it into a behavior and add it to the pipeline. For more information see [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) and [Registering Interception](test-markdown_53570dcb-4520-4e42-b64d-84c9222841c0.html).

# More Information #
<a name="interception_moreinfo" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>For more information about using interception and policy injection with Unity, see the following topics:  
+ [Unity Interception Techniques](test-markdown_9765b670-328a-488c-a219-d114381b7c75.html). This topic describes how interception works in Unity, and provides details of the different interceptors that Unity provides. 
+ [Policy Injection Matching Rules](test-markdown_412f3261-e0f5-4998-8373-5dc2ebda16af.html). This topic describes in detail all of the matching rules included with Unity, and how you can use them to select classes and class members as targets for interception.
+ [Attribute-Driven Policies](test-markdown_456aac54-4ba3-4904-adae-36fb5227fabc.html). This topic explains how you can apply attributes directly to your classes to specify interception and add call handlers to a pipeline. 
+ [Enterprise Library Call Handlers](test-markdown_969b6f02-4da3-41d1-8527-c9e0009d1632.html). This topic describes the call handlers included in Enterprise Library that you can use with policy injection. 
+ [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2.html). This topic describes how to configure interception support and interception policies using configuration files and other configuration sources.
+ [Registering Policy Injection Components](test-markdown_2090aa6d-38c7-4527-a211-aa4fa966e855.html) in the section [Run-Time Configuration](test-markdown_38639e6f-345a-4049-8015-f670458dccad.html). This topic describes how to configure interception support and interception policies at run time using code.
+ [Creating Policy Injection Matching Rules](test-markdown_17137b99-e7a8-4944-b784-87db6ab429af.html). This topic describes how to create your own matching rules if none of those provided with Unity meet your requirements.
+ [Creating Interception Policy Injection Call Handlers](test-markdown_587afe3d-d6c7-447f-bb9b-8fd750174bcc.html). This topic describes the design of call handlers, and how you can create your own. It explains how to handle calls within the pipeline, and how to manage exceptions and errors.
+ [Creating Interception Handler Attributes](test-markdown_f9822e7e-003d-482d-9d72-5f795704367a.html). This topic describes how to create attributes that apply call handlers to classes and their members. It is appropriate if you create your own call handlers.

