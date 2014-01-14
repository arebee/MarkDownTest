---
Source File Name: 75-Interception.docx
AssetID: ed7289ac-2407-409a-8a4d-04bc2a0c763a
Title: Scenarios for Interception
Order In ToC: 2\1
Output Filename: 2\1_Scenarios for Interception.markdown
---

#### Markdown Test ####
# Scenarios for Interception #
----------


> ![(../images/note.gif)#!155CharTopicSummary!#:
> 
Unity addresses a number of interception scenarios.

Unity interception addresses the following scenarios:  
+ Adding responsibilities to individual objects and not the entire class and avoiding a static solution, much as in a decorator pattern. In a manner similar to the way a decorator forwards requests to the object and enables you to perform additional actions before or after forwarding the request, interception intercepts the call to the target object and dynamically adds behaviors to individual objects without affecting any other objects. This can be useful in managing crosscutting concerns that access common features such as logging or validation.
+ To augment or modify the behavior from existing classes that you cannot modify, provided that they are interceptable by the available interception mechanisms.
+ Enabling the developer and administrator to configure the behavior of objects in an application through configuration when used in conjunction with a dependency injection (DI) container, by adding or removing behaviors that execute common tasks or add custom features.
+ Enabling the developer and administrator to capture calls to objects and add or remove behaviors that execute common tasks or add custom features at run time, but in this case independent of a DI container.
+ Minimizing the work required and the code that the developer must write to perform common tasks within an application, such as logging, validation, authorization, and instrumentation.
+ Reducing development time and cost, and minimizing bugs in complex applications that use common and shared tasks and services.

# Benefits of Using Unity Interception #
The benefits of using Unity to perform interception are the following:  
+ It enables the use of alternative interception techniques, such as interface interception and the creation of derived classes, rather than relying only on the Microsoft .NET Framework remoting proxy approach. This can reduce the performance impact of interception.
+ It allows you to resolve objects that have integrated to the container, so you do not need to perform additional steps. All of their dependencies are populated automatically, and any interception functionality is automatically applied to them. For example, it can automatically resolve dependent objects and use them to populate constructor parameters, method parameters, and properties; which considerably reduces the programming effort required.
+ It enables you to use all of the features of interception and the different mechanisms, such as configuration, attributes, and API calls, to specify how to use the interception features.
+ You can specify how to use Unity interception features at design time or at run time. 

The following are factors to keep in mind when evaluating the use of Unity interception to capture calls to objects and add functionality:  
+ It provides a ready-built solution that is easy to implement in new and existing applications, particularly in applications that already take advantage of Unity.
+ It provides a solution that allows developers, operators, and administrators to create, modify, remove, and fine-tune object behaviors though configuration, generally without requiring any changes to the code or recompilation of the application. This limits the chances of introducing errors into the code, simplifies versioning, and reduces downtime.
+ It uses best practice techniques to implement the core features, yet it is flexible and extensible so that developers can create custom behaviors, and even completely replace the default interception mechanism, in order to introduce the behaviors or the interception mechanism they prefer.
+ It enables developers to reuse existing object instances. This reduces the requirement for code to generate new object instances and prepare them by setting properties or calling methods.
+ The interception techniques are intelligent, in that they can examine the current application configuration to determine the desired interceptor and to build a behavior pipeline. 
+ Unity interception provides both instance interceptors and type interceptors.
+ You can apply interception at run time in addition to design-time configuration, which enables your code to respond to environmental changes or other factors.
+ Interception provides more flexibility than other alternatives, such as static inheritance. 

# Limitations of Unity Interception #
All interception technologies impose some extra processing requirements on applicationsâ€”although the design of the Unity mechanism minimizes these as much as possible. The features it provides, and the opportunities it offers to simplify coding solutions that minimize crosscutting concerns and promote manageability, usually outweigh the small extra processing requirement.   
There are some functional limitations when using Unity interception to intercept calls and route them through the behaviors pipeline instead of directly executing custom application code:  
+ Only virtual method interception allows for intercepting and adding functionality to non-public methods and they must be non-final, virtual instance methods.
+ There are some limitations on the type of objects that can be intercepted, depending on the interception mechanism you use. In general, objects must implement a known interface containing the methods and properties for which behaviors are required, inherit from the abstract base class **MarshalByRefObject**, or expose virtual methods that can be overridden in a derived class.
+ The interception mechanism does not reuse behavior instances. They are retrieved every time they are required. Behaviors could be shared if they are explicitly configured as singletons in the container. Although behaviors are not resolved from the container, you could reuse a behavior instance when configuring different types in a container resulting in a shared behavior and, since behaviors apply to every intercepted method that gets invoked, concurrency issues are more likely.
+ Self calls, that is, calls in which a (potentially different) method on the same class is invoked, are not intercepted except when you are using virtual method interception.

# Alternatives to Using Unity Interception #
Alternatives to using Unity to intercept objects may include the following:  
+ One of the aspect-oriented programming (AOP) frameworks available from third-party suppliers, such as the Spring Framework ([http://www.springframework.net/](http://www.springframework.net/))
+ Traditional techniques for adding features such as logging, caching, authorization and validation to the members of a classâ€”for example, using custom code or the ASP.NET server controls

# More Information #
For more information about Unity interception and policy injection, see the following topics:  
+ [Using Interception and Policy Injection](test-markdown_7a2c7fa6-28c2-479e-8df9-b4651824eb94.html)
+ [Unity Interception Techniques](test-markdown_9765b670-328a-488c-a219-d114381b7c75.html)
+ [Policy Injection Matching Rules](test-markdown_412f3261-e0f5-4998-8373-5dc2ebda16af.html)
+ [Attribute-Driven Policies](test-markdown_456aac54-4ba3-4904-adae-36fb5227fabc.html)


