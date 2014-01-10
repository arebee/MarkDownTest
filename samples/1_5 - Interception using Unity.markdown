---
Source File Name: 50-InterceptionWithUnity.docx
AssetID: a1b46b7c-eb1c-4d14-8c9e-0ad2926f5099
Title: 5 - Interception using Unity
Order In ToC: 1
Output Filename: 1_5 - Interception using Unity.markdown
---

#### Markdown Test ####
# 5 - Interception using Unity #
----------


&gt; ![](/images/note.gif)#!155CharTopicSummary!#:
&gt; 
This chapter introduces the concepts around interception with Unity, explains its benefits and drawbacks, discusses some alternatives, and when you should use it.


# Introduction #
Chapter 4 describes interception as a technique that you can use to dynamically insert code that provides support for crosscutting concerns into your application without explicitly using the decorator pattern in your code. In this chapter, you'll learn how you can use Unity to implement interception, and the various types of interception that Unity supports. The chapter starts by describing a common scenario for using interception and illustrates how you can implement it using Unity interception. The chapter then explores a number of alternative approaches that you could adopt, including the use of policy injection and attributes.  

&gt; ![](/images/note.gif)Note:
&gt; You cannot use Unity Interception in Windows Store Applications. For more information, see [Appendix A – Unity and Windows Store apps](test-markdown_9e9684e7-1116-4a72-9a61-963d9803440c.html).


# Crosscutting Concerns and Enterprise Library  #
This chapter begins by describing how you can use Unity interception to implement your own custom code to add support for crosscutting concerns into your application. However, the blocks in Enterprise Library provide rich support for crosscutting concerns, and therefore it’s not surprising that instead of your own custom code you can use the Enterprise Library blocks with Unity interception; the chapter also describes this approach.  

&gt; ![](/images/note.gif)#!SPOKENBY(Jana)!#:
&gt; 
You can use policy injection and call handlers to integrate the functionality provided by the Enterprise Library blocks with Unity interception.


# Interceptors in Unity #
This section describes the basic steps that you’ll need to complete in order to use interception in your application. To illustrate some of the advantages of the interception approach, this chapter uses the same example crosscutting concerns, logging and caching, that the discussion of the decorator pattern in Chapter 4 uses. You may find it useful to refer back to Chapter 4, "[Interception](test-markdown_19429ab2-0050-486e-81ed-2db92281aca8.html)," for some of the details that relate to the logging and caching functionality.   

&gt; ![](/images/note.gif)Note:
&gt; This section uses logging as an example of a crosscutting concern and shows how you can implement it using interception. Later in this chapter, you’ll see how you can use the Enterprise Library logging block in place of your own implementation.


## Configuring the Unity Container to Support Interception ##
By default, a Unity container does not support interception; to add support for interception you must add the Unity interception extension as shown in the following code sample. You can add the Unity Interception Extension libraries to your project using NuGet.  

```csharp
using Microsoft.Practices.Unity.InterceptionExtension;

...

IUnityContainer container = new UnityContainer();
container.AddNewExtension&lt;Interception&gt;();
```


&gt; ![](/images/note.gif)#!SPOKENBY(Jana)!#:
&gt; 
If you don’t need to use Unity interception, you don’t need to install the libraries through NuGet: interception is an extension to the core Unity installation.
For more information about how to add the interception extension to a Unity container instance and about how you can use a configuration file instead of code, see the topic [Configuring a Container for Interception](http://go.microsoft.com/fwlink/p/?LinkID=304182).  


## Defining an Interceptor ##
In Chapter 4, the two example crosscutting concerns in the decorator example were logging and caching. The example showed implementations of these behaviors in classes that implemented the **ILogger** and **ICacheManager** interfaces. To implement these behaviors as interceptors, you need classes that implement the **IInterceptionBehavior** interface. The following example shows how you can implement the logging behavior.  

```csharp
using Microsoft.Practices.Unity.InterceptionExtension;

class LoggingInterceptionBehavior : IInterceptionBehavior
{
  public IMethodReturn Invoke(IMethodInvocation input,
    GetNextInterceptionBehaviorDelegate getNext)
  {
    // Before invoking the method on the original target.
    WriteLog(String.Format(
      "Invoking method {0} at {1}",
      input.MethodBase, DateTime.Now.ToLongTimeString()));

    // Invoke the next behavior in the chain.
    var result = getNext()(input, getNext);

    // After invoking the method on the original target.
    if (result.Exception != null)
    {
      WriteLog(String.Format(
        "Method {0} threw exception {1} at {2}",
        input.MethodBase, result.Exception.Message,
        DateTime.Now.ToLongTimeString()));
    }
    else
    {
      WriteLog(String.Format(
        "Method {0} returned {1} at {2}",
        input.MethodBase, result.ReturnValue,
        DateTime.Now.ToLongTimeString()));
    }
    
    return result;
  }

  public IEnumerable&lt;Type&gt; GetRequiredInterfaces()
  {
    return Type.EmptyTypes;
  }

  public bool WillExecute
  {
    get { return true; }
  }

  private void WriteLog(string message)
  {
    ...
  }
}
```

The **IInterceptionBehavior** interface defines three methods: **WillExecute**, **GetRequiredInterfaces**, and **Invoke**. In many scenarios, you can use the default implementations of the **WillExecute** and **GetRequiredInterfaces** methods shown in this example. The **WillExecute** method enables you to optimize your chain of behaviors by specifying whether this behavior should execute; in this example the method always returns **true** so the behavior always executes. The **GetRequiredInterfaces** method enables you to specify the interface types that you want to associate with the behavior. In this example, the interceptor registration will specify the interface type, and therefore the **GetRequiredInterfaces** method returns **Type.EmptyTypes**. For more information about how to use these two methods, see the topic [Behaviors for Interception](http://go.microsoft.com/fwlink/p/?LinkID=304183).  
The **Invoke** method takes two parameters: **input** contains information about the call from the client that includes the method name and parameter values, **getNext** is a delegate that enables you to call the next behavior in the pipeline, or the target object if this is the last behavior in the pipeline. In this example, you can see how the behavior captures the name of the method invoked by the client and records details of the method invocation in the log before it invokes the next behavior in the pipeline, or target object itself. This behavior then examines the result of the call to the next behavior in the pipeline and writes a log message if an exception was thrown in the call, or details of the result if the call succeeded. Finally, it returns the result of the call back to the previous behavior in the pipeline (or the client object if this behavior was first in the pipeline).  

&gt; ![](/images/note.gif)#!SPOKENBY(Markus)!#:
&gt; 
In the **Invoke** method, you can apply pre and post processing to the call to the target object by placing your code before and after the call to the **getNext** delegate.
The following code example shows a slightly more complex behavior that provides support for caching.  

```csharp
using Microsoft.Practices.Unity.InterceptionExtension;

class CachingInterceptionBehavior : IInterceptionBehavior
{
  public IMethodReturn Invoke(IMethodInvocation input,
    GetNextInterceptionBehaviorDelegate getNext)
  {
    
    // Before invoking the method on the original target.
    if (input.MethodBase.Name == "GetTenant")
    {
      var tenantName = input.Arguments["tenant"].ToString();
      if (IsInCache(tenantName))
      {
        return input.CreateMethodReturn(
          FetchFromCache(tenantName));
      }
    }

    IMethodReturn result = getNext()(input, getNext);

    // After invoking the method on the original target.
    if (input.MethodBase.Name == "SaveTenant")
    {
      AddToCache(input.Arguments["tenant"]);
    }

    return result;

  }

  public IEnumerable&lt;Type&gt; GetRequiredInterfaces()
  {
    return Type.EmptyTypes;
  }

  public bool WillExecute
  {
    get { return true; }
  }

  private bool IsInCache(string key) {...}

  private object FetchFromCache(string key) {...}

  private void AddToCache(object item) {...}
}
```

This behavior filters for calls to a method called **GetTenant** and then attempts to retrieve the named tenant from the cache. If it finds the tenant in the cache, it does not need to invoke the target object to get the tenant, and instead uses the **CreateMethodReturn** to return the tenant from the cache to the previous behavior in the pipeline.  

&gt; ![](/images/note.gif)#!SPOKENBY(Carlos)!#:
&gt; 
Behaviors do not always have to pass on a call to the next behavior in the pipeline. They can generate the return value themselves or throw an exception.
The behavior also filters for a method called **SaveTenant** after it has invoked the method on the next behavior in the pipeline (or the target object) and adds to the cache a copy of the tenant object saved by the target object.  

&gt; ![](/images/note.gif)Note:
&gt; This example embeds a filter that determines when the interception behavior should be applied. Later in this chapter, you’ll see how you replace this with a policy that you can define in a configuration file or with attributes in your code.


## Registering an Interceptor ##
Although it is possible to use behaviors such as the **CachingInterceptionBehavior** and **LoggingInterceptionBehavior** without the Unity container, the following code sample shows how you can use the container to register the interception behaviors to the **TenantStore** type so that the container sets up the interception whenever it resolves the type.  

```csharp
using Microsoft.Practices.Unity.InterceptionExtension;

...

container.AddNewExtension&lt;Interception&gt;();
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new Interceptor&lt;InterfaceInterceptor&gt;(),
  new InterceptionBehavior&lt;LoggingInterceptionBehavior&gt;(),
  new InterceptionBehavior&lt;CachingInterceptionBehavior&gt;());
```

The first parameter to the **RegisterType** method, an **Interceptor&lt;InterfaceInterceptor&gt;** object, specifies the type of interception to use. In this example, the next two parameters register the two interception behaviors with the **TenantStore** type.  

&gt; ![](/images/note.gif)Note:
&gt; The **InterfaceInterceptor** type defines interception based on a proxy object. You can also use the **TransparentProxyInterceptor** and **VirtualMethodInterceptor** types that are described later in this chapter.
The order of the interception behavior parameters determines the order of these behaviors in the pipeline. In this example, the order is important because the caching interception behavior does not pass on the request from the client to the next behavior if it finds the item in the cache. If you reversed the order of these two interception behaviors, you wouldn’t get any log messages if the requested item was found in the cache.  

&gt; ![](/images/note.gif)#!SPOKENBY(Jana)!#:
&gt; 
Placing all your registration code in one place means that you can manage all the interception behaviors in your application from one location.


## Using an Interceptor ##
The final step is to use the interceptor at run time. The following code sample shows how you can resolve and use a **TenantStore** object that has the logging and caching behaviors attached to it.  

```csharp
var tenantStore = container.Resolve&lt;ITenantStore&gt;();
tenantStore.SaveTenant(tenant);
```

The type of the **tenantStore** variable is not **TenantStore**, it is a new dynamically created proxy type that implements the **ITenantStore** interface. This proxy type includes the methods, properties, and events defined in the **ITenantStore** interface.  
Figure 1 illustrates the scenario implemented by the two behaviors and type registration you’ve seen in the previous code samples.  
![](/images/images\7708FE4F651AB54391D028161B98FD03.png)  
<p class="label" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><div class="caption">Figure 1 - The behavior pipeline</div></p>The numbers in the following list correspond to the numbers in Figure 1.  
1. The client object calls the **Resolve** method on the Unity container to obtain an object that implements the **ITenantStore** interface. The container uses the **ITenantStore** registration information to instantiate a **TenantStore** object, create a pipeline with the interception behaviors, and dynamically generate a proxy **TenantStore** object. 
2. The client invokes the **GetTenant** method on the **TenantStore** proxy object, passing a single string parameter identifying the tenant to fetch from the store.
3. The **TenantStore** proxy object, calls the **Invoke** method in the first interception behavior. This logging interception behavior logs details of the call.
4. The first interception behavior, calls the **Invoke** method in the next behavior in the pipeline. If the caching behavior finds the tenant in the cache, jump to step 7.
5. The last interception behavior, calls the **GetTenant** method on the **TenantStore** object.
6. The **TenantStore** object returns a tenant to the last interception behavior in the pipeline.
7. If the caching interception behavior found the tenant in the cache it returns the cached tenant, otherwise it returns the tenant from the **TenantStore** object and caches it.
8. The logging interception behavior logs details of the result of the call that it made to the next behavior in the pipeline and returns the result back to the **TenantStore** proxy object.
9. The **TenantStore** proxy object returns the tenant object to the client object.

&gt; ![](/images/note.gif)#!SPOKENBY(Carlos)!#:
&gt; 
The interceptors get access to the parameters passed to the original call, the value returned from the original call, and any exceptions thrown by the original call.


# Alternative Interception Techniques #
The example shown earlier in this chapter illustrated the most common way to use interception in Unity. This section discusses some variations on the basic approach and suggests some scenarios where you might wish to use them.  


## Instance Interception/Type Interception ##
The approach used in the example earlier in this chapter is an example of instance interception where Unity dynamically generates a proxy object and enables interception of methods defined in an interface, in this example the **ITenantStore** interface. As a reminder, here is the code that registers the interception.  

```csharp
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new Interceptor&lt;InterfaceInterceptor&gt;(),
  new InterceptionBehavior&lt;LoggingInterceptionBehavior&gt;(),
  new InterceptionBehavior&lt;CachingInterceptionBehavior&gt;());
```

In this example, the **Interceptor&lt;InterfaceInterceptor&gt;** parameter specifies you are using a type of instance interception known as interface interception, and the **InterceptionBehavior** parameters define the two behaviors to insert into the behavior pipeline.  
Unity interception includes two other interceptor types: **TransparentProxyInterceptor** and **VirtualMethodInterceptor**.  
The following table summarizes the available interceptor types:  
![](/images/images\3E1DEE59E65E54684DF57B5C2FF41A53.png)  
For more information about the different interceptor types, see the topic [Unity Interception Techniques](http://go.microsoft.com/fwlink/p/?LinkID=304181).  


### Using the TransparentProxyInterceptor Type ###
The **InterfaceInterceptor** type enables you to intercept methods on only one interface; in the example above, you can intercept the methods on the **ITenantStore** interface. If the **TenantStore** class also implements an interface such as the **ITenantLogoStore** interface, and you want to intercept methods defined on that interface in addition to methods defined on the **ITenantStore** interface then you should use the **TransparentProxyInterceptor** type as shown in the following code sample.  

```csharp
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new Interceptor&lt;TransparentProxyInterceptor&gt;(),
  new InterceptionBehavior&lt;LoggingInterceptionBehavior&gt;(),
  new InterceptionBehavior&lt;CachingInterceptionBehavior&gt;());

var tenantStore = container.Resolve&lt;ITenantStore&gt;();

// From the ITenantStore interface.
tenantStore.SaveTenant(tenant);

// From the ITenantLogoStore interface.
((ITenantLogoStore)tenantStore).SaveLogo("adatum", logo);
```

The drawback to using the **TransparentProxyInterceptor** type in place of the **InterfaceInterceptor** type is that it is significantly slower at run time. You can also use this approach if the **TenantStore** class doesn’t implement any interfaces but does extend the **MarshalByRef** abstract base class.  

&gt; ![](/images/note.gif)#!SPOKENBY(Carlos)!#:
&gt; 
Although the **TransparentProxyInterceptor** type appears to be more flexible than the **InterfaceInterceptor** type, it is not nearly as performant.


### Using the VirtualMethodInterceptor Type ###
The third interceptor type is the **VirtualMethodInterceptor** type. If you use the **InterfaceInterceptor** or **TransparentProxyInterceptor** type, then at run time Unity dynamically creates a proxy object. However, the proxy object is not type compatible with the target object. In the previous code sample, the **tenantStore** object is not an instance of, or derived from, the **TenantStore** class.   
In the example shown below, which uses the **VirtualMethodInterceptor** interception type, the type of the **tenantStore** object derives from the **TenantStore** type so you can use it wherever you can use a **TenantStore** instance. However, you cannot use the **VirtualMethodInterceptor** interceptor on existing objects, you can only configure type interception when you are creating the target object. For example, you cannot use type interception on a WCF service proxy object that is created for you by a channel factory. For the logging and caching interception behaviors to work when you invoke the **SaveTenant** method in the following example, the **SaveTenant** method in the **TenantStore** class must be virtual, and the **TenantStore** class must be public.  

```csharp
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new Interceptor&lt;VirtualMethodInterceptor&gt;(),
  new InterceptionBehavior&lt;LoggingInterceptionBehavior&gt;(),
  new InterceptionBehavior&lt;CachingInterceptionBehavior&gt;());

var tenantStore = container.Resolve&lt;ITenantStore&gt;();

tenantStore.SaveTenant(tenant);
```


&gt; ![](/images/note.gif)Note:
&gt; If you use virtual method interception, the container generates a new type that directly extends the type of the target object. The container inserts the behavior pipeline by overriding the virtual methods in the base type.
Another advantage of virtual method interception is that you can intercept internal calls in the class that happen when one method in a class invokes another method in the same class. This is not possible with instance interception.  

&gt; ![](/images/note.gif)#!SPOKENBY(Jana)!#:
&gt; 
Calls made with the **VirtualMethodInterceptor** are much faster than those made with the **TransparentProxyInterceptor**. 
Figure 2 shows the objects that are involved with interception behaviors in this example.  
![](/images/images\D3C12313037D0B7076CFCC38E993A8CE.png)  
<p class="label" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><div class="caption">Figure 2 - Interception using the virtual method interceptor</div></p>The numbers in the following list correspond to the numbers in Figure 2.  
1. The client object calls the **Resolve** method on the Unity container to obtain an object that implements the **ITenantStore** interface. The container uses the **ITenantStore** registration information to instantiate an object from a class derived from the **TenantStore** type that includes a pipeline with the interception behaviors. 
2. The client invokes the **GetTenant** method on the **TenantStore** derived object, passing a single string parameter identifying the tenant to fetch from the store.
3. The **TenantStore** derived object, calls the **Invoke** method in the first interception behavior. This logging interception behavior logs details of the call.
4. The first interception behavior, calls the **Invoke** method in the next behavior in the pipeline. If the caching behavior finds the tenant in the cache, jump to step 7.
5. The last interception behavior, calls the **GetTenant** method in the **TenantStore** base class.
6. The **TenantStore** base class returns a tenant to the last interception behavior in the pipeline.
7. If the caching interception behavior found the tenant in the cache it returns the cached tenant, otherwise it returns the tenant from the **TenantStore** object.
8. The logging interception behavior logs details of the result of the call that it made to the next behavior in the pipeline and returns the result back to the **TenantStore** derived object.
9. The **TenantStore** derived object returns the tenant object to the client object.
For more information about the different interceptor types, see [Unity Interception Techniques](http://go.microsoft.com/fwlink/p/?LinkID=304181).  


## Using a Behavior to Add an Interface to an Existing Class ##
Sometimes it is useful to be able to add an interface implementation to an existing class without changing that class. For example, the following **TenantStore** class only implements the **ITenantStore** interface.  

```csharp
public class TenantStore : ITenantStore
{
  ...
}
```

However, you may have a requirement for **TenantStore** instances to implement some custom logging methods such as **WriteLogMessage** defined in the **ILogger** interface. This requirement may be in addition to that of using interception to write log messages when you invoke methods, such as **GetTenant** or **SaveTenant,** on the **TenantStore** class. Using a behavior, you can make it possible to write code such as that shown in the following example without modifying the original **TenantStore** class.  

```csharp
((ILogger)tenantStore).WriteLogMessage("Message: Write to the log directly...");
```

When you register the **TenantStore** type with the container, you can specify that it supports additional interfaces, such as the **ILogger** interface, as shown in the following example:  

```csharp
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new Interceptor&lt;InterfaceInterceptor&gt;(),
  new InterceptionBehavior&lt;LoggingInterceptionBehavior&gt;(),
  new InterceptionBehavior&lt;CachingInterceptionBehavior&gt;(),
  new AdditionalInterface&lt;ILogger&gt;());
```

Then, in a behavior, you can intercept any calls made to methods defined on that interface. The following code sample shows part of the **LoggingInterceptionBehavior** class that filters for calls to methods on the **ILogger** interface and handles them.  

```csharp
class LoggingInterceptionBehavior : IInterceptionBehavior
{
  public IMethodReturn Invoke(IMethodInvocation input,
    GetNextInterceptionBehaviorDelegate getNext)
  {
    var methodReturn = CheckForILogger(input);

    if (methodReturn != null)
    {
      return methodReturn;
    }

    ...
  }

  ...

  private IMethodReturn CheckForILogger(IMethodInvocation input)
  {
    if (input.MethodBase.DeclaringType == typeof(ILogger)
         &amp;&amp; input.MethodBase.Name == "WriteLogMessage")
    {
      WriteLog(input.Arguments["message"].ToString());
      return input.CreateMethodReturn(null);
    }
    return null;
  }
}
```

Note how in this example the behavior intercepts the call to the **WriteLogMessage** interface and does not forward it on to the target object. This is because the target object does not implement the **ILogger** interface and does not have a **WriteLogMessage** method.  


## Interception Without the Unity Container ##
The examples you’ve seen so far show how to setup interception as part of the type registration in the Unity container. If you are not using a Unity container for DI or want to use interception without using a container, you can use the **Intercept** class. The following two examples show how you can setup interception using the Unity container, and using the **Intercept** class. Both examples attach the same interception pipeline to a **TenantStore** object.  

```csharp
// Example 1. Using a container.
// Configure the container for interception.
container = new UnityContainer();
container.AddNewExtension&lt;Interception&gt;();

// Register the TenantStore type for interception.
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new Interceptor&lt;InterfaceInterceptor&gt;(),
  new InterceptionBehavior&lt;LoggingInterceptionBehavior&gt;(),
  new InterceptionBehavior&lt;CachingInterceptionBehavior&gt;());

// Obtain a proxy object with an interception pipeline.
var tenantStore = container.Resolve&lt;ITenantStore&gt;();
```


```csharp
// Example 2. Using the Intercept class.
ITenantStore tenantStore = Intercept.ThroughProxy&lt;ITenantStore&gt;(
  new TenantStore(tenantContainer, blobContainer),
  new InterfaceInterceptor(),
  new IInterceptionBehavior [] { 
    new LoggingInterceptionBehavior(), new CachingInterceptionBehavior()
  });
```

It is sometimes convenient to use stand-alone interception and the **Intercept** class if you want to attach an intercept pipeline to an existing object as shown in the following example.  

```csharp
TenantStore tenantStore =
  new TenantStore(tenantContainer, blobContainer);

...

// Attach an interception pipeline.
ITenantStore proxyTenantStore = Intercept.ThroughProxy&lt;ITenantStore&gt;(
  tenantStore,
  new InterfaceInterceptor(),
    new IInterceptionBehavior [] {
      new LoggingInterceptionBehavior(), new CachingInterceptionBehavior()
  });
```

You can use the **ThroughProxy** methods of the **Intercept** class to set up instance interception that uses a proxy object , and the **NewInstance** methods to set up type interception that uses a derived object. You can only attach an interception pipeline to an existing object if you use the **ThroughProxy** methods; the **NewInstance** methods always create a new instance of the target object.  

&gt; ![](/images/note.gif)#!SPOKENBY(Carlos)!#:
&gt; 
You don’t need to use a Unity container if you want to use interception in your application.
For more information, see [Stand-alone Unity Interception](http://go.microsoft.com/fwlink/p/?LinkID=304185).  
<a name="_Design_Time_Configuration" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Design Time Configuration ##
All of the examples so far in this chapter have configured interception programmatically, either in a Unity container or by using the **Intercept** class. However, in the same way that you can define your type registrations for the container in a configuration file, you can also define your interception behavior pipeline.  
You’ve seen the following registration code for the **TenantStore** class several times already in this chapter.  

```csharp
// Configure the container for interception.
container = new UnityContainer();
container.AddNewExtension&lt;Interception&gt;();

// Register the TenantStore type for interception.
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new Interceptor&lt;InterfaceInterceptor&gt;(),
  new InterceptionBehavior&lt;LoggingInterceptionBehavior&gt;(),
  new InterceptionBehavior&lt;CachingInterceptionBehavior&gt;());

// Obtain a proxy object with an interception pipeline.
var tenantStore = container.Resolve&lt;ITenantStore&gt;();
```

Instead of this programmatic configuration, you can add the following configuration information to your configuration file.  

```other
&lt;unity xmlns="http://schemas.microsoft.com/practices/2010/unity"&gt;
  &lt;sectionExtension type=
    "Microsoft.Practices.Unity.InterceptionExtension
    .Configuration.InterceptionConfigurationExtension,
    Microsoft.Practices.Unity.Interception.Configuration" /&gt;
  &lt;namespace name="Tailspin.Web.Survey.Shared.Stores" /&gt;
  &lt;namespace name="Tailspin.Utilities.InterceptionBehaviors" /&gt;
  &lt;assembly name="Tailspin.Web.Survey.Shared" /&gt;
  &lt;assembly name="Tailspin.Utilities" /&gt;
  &lt;container&gt;
    &lt;register type="ITenantStore" mapTo="TenantStore"&gt;
      &lt;interceptor type="InterfaceInterceptor"/&gt;
      &lt;interceptionBehavior type="LoggingInterceptionBehavior" /&gt;
      &lt;interceptionBehavior type="CachingInterceptionBehavior" /&gt;
    &lt;/register&gt;
  &lt;/container&gt;
&lt;/unity&gt;
```

Note that this snippet from the configuration file includes a **sectionExtension** element to enable you to use the interception specific elements. There are also **namespace** and **assembly** elements to enable Unity to find your interception behavior classes in addition to the **TenantStore** class and **ITenantStore** interface.  
The following code sample shows how you can load this configuration information and then resolve a **TenantStore** instance with an interception pipeline that includes the logging and caching behaviors.  

```csharp
IUnityContainer container = new UnityContainer();
container.LoadConfiguration();

// Obtain a proxy object with an interception pipeline.
var tenantStore = container.Resolve&lt;ITenantStore&gt;();

...

tenantStore.UploadLogo("tenant", logo);
```


&gt; ![](/images/note.gif)#!SPOKENBY(Poe)!#:
&gt; 
Defining the interception behaviors in the configuration file makes it possible to change their configuration without recompiling the code.


# Policy Injection #
The following code sample shows how you insert two behaviors into the pipeline for a **TenantStore** instance created by the Unity container.  

```csharp
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new Interceptor&lt;InterfaceInterceptor&gt;(),
  new InterceptionBehavior&lt;LoggingInterceptionBehavior&gt;(),
  new InterceptionBehavior&lt;CachingInterceptionBehavior&gt;());
```

One of the drawbacks of this approach to implementing support for crosscutting concerns is that you must configure the interceptors for every class that needs them. In practice, you may have additional store classes in your application that all require caching support, and many more that require logging behavior.  

&gt; ![](/images/note.gif)#!SPOKENBY(Jana)!#:
&gt; 
You may be able to use registration by convention to configure interceptors for multiple registered types.
The following code sample shows an alternative approach based on policies for adding interception behaviors to objects in your application. Note that definitions of the **LoggingCallHandler** and **CachingCallHandler** classes are given later in this section.  

```csharp
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new InterceptionBehavior&lt;PolicyInjectionBehavior&gt;(),
  new Interceptor&lt;InterfaceInterceptor&gt;());

container.RegisterType&lt;ISurveyStore, SurveyStore&gt;(
  new InterceptionBehavior&lt;PolicyInjectionBehavior&gt;(),
  new Interceptor&lt;InterfaceInterceptor&gt;());

var first = new InjectionProperty("Order", 1);
var second = new InjectionProperty("Order", 2);

container.Configure&lt;Interception&gt;()
  .AddPolicy("logging")
  .AddMatchingRule&lt;AssemblyMatchingRule&gt;(
    new InjectionConstructor(
    new InjectionParameter("Tailspin.Web.Survey.Shared")))
  .AddCallHandler&lt;LoggingCallHandler&gt;(
    new ContainerControlledLifetimeManager(),
    new InjectionConstructor(),
    first);

container.Configure&lt;Interception&gt;()
  .AddPolicy("caching")
  .AddMatchingRule&lt;MemberNameMatchingRule&gt;(
    new InjectionConstructor(new [] {"Get*", "Save*"}, true))
  .AddMatchingRule&lt;NamespaceMatchingRule&gt;(
    new InjectionConstructor(
      "Tailspin.Web.Survey.Shared.Stores", true))
  .AddCallHandler&lt;CachingCallHandler&gt;(
    new ContainerControlledLifetimeManager(),
    new InjectionConstructor(),
    second);
```

This example registers two store types with the container using a **PolicyInjectionBehavior** behavior type. This behavior makes use of policy definitions to insert handlers into a pipeline when a client calls an object instantiated by the container. The example shows two policy definitions: one for the **LoggingCallHandler** interception handler and one for the **CachingCallHandler **interception handler. Each policy has a name to identify it and one or more matching rules to determine when to apply it.   

&gt; ![](/images/note.gif)#!SPOKENBY(Markus)!#:
&gt; 
You must remember to configure policy injection behavior for each type that you want to use it on as well as defining the policies.
The Policy Injection Application Block has built-in matching rules based on the following:  
+ Assembly name
+ Namespace
+ Type
+ Tag attribute
+ Custom attribute
+ Member name
+ Method signature
+ Parameter type
+ Property
+ Return type

&gt; ![](/images/note.gif)Note:
&gt; For more information about these matching rules and how to use them, see the topic [Policy Injection Matching Rules](http://go.microsoft.com/fwlink/p/?LinkID=304186). 
You can also define your own, custom matching rule types. For more information, see the topic [Creating Policy Injection Matching Rules](http://go.microsoft.com/fwlink/p/?LinkID=304187).
The policy for logging has a single matching rule based on the name of the assembly that contains the class definition of the target object: in this example, if the **TenantStore** and **SurveyStore** classes are located in the **Tailspin.Web.Survey.Shared** assembly, then the handler will log all method calls to those objects.  
The policy for caching uses two matching rules that are ANDed together: in this example, the caching handler handles all calls to methods that start either with **Get** or **Save** (the **true** parameter makes it a case sensitive test), and that are in the **Tailspin.Web.Survey.Shared.Stores** namespace.  

&gt; ![](/images/note.gif)#!SPOKENBY(Carlos)!#:
&gt; 
You can use the policy injection matching rules to avoid the need embed filters in an injection behavior class.
The **first** and **second** parameters control the order that the container invokes the handlers if the matching rules match multiple handlers to a single instance. In this example, you want to be sure that the container invokes the logging handler before the caching handler in order to ensure that all calls are logged.  

&gt; ![](/images/note.gif)Note:
&gt; Remember, in the example used in this chapter, the caching handler does not pass on the call if it locates the item in the cache.
Both policies use the **ContainerControlledLifetimeManager** type to ensure that the handlers are singleton objects in the container.  
The handler classes are very similar to the behavior interception classes you saw earlier in this chapter. The following code samples shows the logging handler and the cache handler used in the examples shown in this section.  

```csharp
class LoggingCallHandler : ICallHandler
{
  public IMethodReturn Invoke(IMethodInvocation input,
    GetNextHandlerDelegate getNext)
  {
    // Before invoking the method on the original target
    WriteLog(String.Format("Invoking method {0} at {1}",
      input.MethodBase, DateTime.Now.ToLongTimeString()));

    // Invoke the next handler in the chain
    var result = getNext().Invoke(input, getNext);

    // After invoking the method on the original target
    if (result.Exception != null)
    {
      WriteLog(String.Format("Method {0} threw exception {1} at {2}",
        input.MethodBase, result.Exception.Message,
        DateTime.Now.ToLongTimeString()));
    }
    else
    {
      WriteLog(String.Format("Method {0} returned {1} at {2}",
        input.MethodBase, result.ReturnValue,
        DateTime.Now.ToLongTimeString()));
    }

    return result;
  }

  public int Order
  {
    get;
    set;
  }

  private void WriteLog(string message)
  {
    ...
  }
}
```


```csharp
public class CachingCallHandler :ICallHandler

{
  public IMethodReturn Invoke(IMethodInvocation input,
    GetNextHandlerDelegate getNext)
  {
    //Before invoking the method on the original target
    if (input.MethodBase.Name == "GetTenant")
    {
      var tenantName = input.Arguments["tenant"].ToString();
      if (IsInCache(tenantName))
      {
        return input.CreateMethodReturn(FetchFromCache(tenantName));
      }
    }

    IMethodReturn result = getNext()(input, getNext);

    //After invoking the method on the original target
    if (input.MethodBase.Name == "SaveTenant")
    {
      AddToCache(input.Arguments["tenant"]);
    }

    return result;
  }

  public int Order
  {
    get;
    set;
  }

  private bool IsInCache(string key)
  {
    ...
  }

  private object FetchFromCache(string key)
  {
    ...
  }

  private void AddToCache(object item)
  {
    ...
  }
}
```

In many cases, policy injection handler classes may be simpler than interception behavior classes that address the same crosscutting concerns. This is because you can use the policy matching rules to control when the container invokes the handler classes whereas very often with interception behavior classes, such as the **CachingInterceptionBehavior** class shown earlier in the chapter, you need to implement a filter within **Invoke** method to determine whether the behavior should execute in a particular circumstance.  
The **Order** property, which is set when you configure the policy, controls the order in which the container executes the handlers when a policy rule matches more than one handler.  

&gt; ![](/images/note.gif)#!SPOKENBY(Carlos)!#:
&gt; 
You may need to consider the order of the call handlers carefully, especially if some of them don’t always pass the call on to the next handler. 


## Policy Injection and Attributes ##
Using policies with matching rules such as those shown in the previous example means that you can apply and manage the policies when you configure your container. You can define the policies either in code, as shown in the example, or in a configuration file in a similar way to that described in the section “<a href="#_Design_Time_Configuration" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Design Time Configuration</a>” earlier in this chapter.  
However, an alternative approach is to use attributes in your business classes to indicate whether the container should invoke a particular call handler. In this case, the person writing the business class is responsible for making sure that all the crosscutting concerns are addressed.  

&gt; ![](/images/note.gif)#!SPOKENBY(Jana)!#:
&gt; 
Typically, if you define policies for your call handlers, they are all defined in a single class or configuration file where they are easy to manage. Using attributes means that information about which crosscutting concerns are associated with particular classes and methods is stored in those classes which is a less maintainable approach.
In this scenario, you can register the types that the container should inject with policies as shown in the following code sample.  

```csharp
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new InterceptionBehavior&lt;PolicyInjectionBehavior&gt;(),
  new Interceptor&lt;InterfaceInterceptor&gt;());

container.RegisterType&lt;ISurveyStore, SurveyStore&gt;(
  new InterceptionBehavior&lt;PolicyInjectionBehavior&gt;(),
  new Interceptor&lt;InterfaceInterceptor&gt;());
```

There is no need to configure any policies, but you do need to define your attributes. The following code sample shows the **LoggingCallHandlerAttribute** class. The **CachingCallHandlerAttribute** class is almost identical.  

```csharp
using Microsoft.Practices.Unity;
using Microsoft.Practices.Unity.InterceptionExtension;

class LoggingCallHandlerAttribute : HandlerAttribute
{
  private readonly int order;

  public LoggingCallHandlerAttribute(int order)
  {
    this.order = order;
  }

  public override ICallHandler CreateHandler(IUnityContainer container)
  {
    return new LoggingCallHandler() { Order = order };
  }
}
```

The **CreateHandler** method creates an instance of the **LoggingCallHandler** class that you saw previously and sets the value of the **Order** property.  
You can now use the attributes to decorate your business classes as shown in the following example.  

```csharp
class MyTenantStoreWithAttributes : ITenantStore, ITenantLogoStore
{
  ...

  [LoggingCallHandler(1)]
  public void Initialize()
  {
    ...
  }

  [LoggingCallHandler(1)]
  [CachingCallHandler(2)]
  public Tenant GetTenant(string tenant)
  {
    ...
  }

  [LoggingCallHandler(1)]
  public IEnumerable&lt;string&gt; GetTenantNames()
  {
    ...
  }

  [LoggingCallHandler(1)]
  [CachingCallHandler(2)]
  public void SaveTenant(Tenant tenant)
  {
    ...
  }

  [LoggingCallHandler(1)]
  public virtual void UploadLogo(string tenant, byte[] logo)
  {
    ...
  }
}
```

In this example, the **LoggingCallHandler** call handler logs all the method calls, and the **GetTenant** and **SaveTenant** methods also have a caching behavior applied to them. The example uses the **Order** property of the handlers to place the logging call handler before the caching call handler in the pipeline.  


## Policy Injection and the Enterprise Library Blocks ##
In the previous examples, you implemented the caching and logging behaviors in your own call handler classes: **CachingCallHandler** and **LoggingCallHandler**. The blocks in the Enterprise Library, such as the Logging Application Block and the Validation Application Block, provide some pre-written call handlers that you can use in your own applications. This enables you to use the Enterprise Library blocks to address crosscutting concerns in your application using policy injection.  

&gt; ![](/images/note.gif)#!SPOKENBY(Jana)!#:
&gt; 
Using the Enterprise Library blocks to address your cross-cutting concerns is often easier than implementing the behaviors yourself.
For example, you could replace your **LoggingCallHandler** with **LogCallHandler** from the Enterprise Library Policy Injection Application Block as shown in the following code sample. However, Enterprise Library 6 does not include a caching handler.  

```csharp
ConfigureLogger();
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new InterceptionBehavior&lt;PolicyInjectionBehavior&gt;(),
  new Interceptor&lt;InterfaceInterceptor&gt;());

var second = new InjectionProperty("Order", 2);

container.Configure&lt;Interception&gt;()
  .AddPolicy("logging")
  .AddMatchingRule&lt;AssemblyMatchingRule&gt;(
    new InjectionConstructor(
      new InjectionParameter("Tailspin.Web.Survey.Shared")))
  .AddCallHandler&lt;LogCallHandler&gt;(
    new ContainerControlledLifetimeManager(),
    new InjectionConstructor(
      9001, true, true,
      "start", "finish", true, false, true, 10, 1));

container.Configure&lt;Interception&gt;()
  .AddPolicy("caching")
  .AddMatchingRule&lt;MemberNameMatchingRule&gt;(
    new InjectionConstructor(new[] { "Get*", "Save*" }, true))
  .AddMatchingRule&lt;NamespaceMatchingRule&gt;(
    new InjectionConstructor(
      "Tailspin.Web.Survey.Shared.Stores", true))
  .AddCallHandler&lt;CachingCallHandler&gt;(
    new ContainerControlledLifetimeManager(),
    new InjectionConstructor(),
    second);
```

The **LogCallHandler** class from the Logging Application Block is a call handler to use with the policy injection framework.   

&gt; ![](/images/note.gif)#!SPOKENBY(Jana)!#:
&gt; 
It’s important that you configure the Logging Application Block before you configure the policy injection. The sample code in the PIABSamples solution that accompanies this guide includes a method **ConfigureLogger** to do this.
The **LogCallHandler** constructor parameters enable you to configure the logging behavior and specify the order of the handler in relation to other call handlers. In this example, the container will invoke the **LogCallHandler** call handler before the user defined **CachingCallHandler** call handler.  
If you want to control the behavior of the call handlers by using attributes in your business classes instead of through policies, you can enable this approach as shown in the following code sample.  

```csharp
container.RegisterType&lt;ITenantStore, TenantStore&gt;(
  new InterceptionBehavior&lt;PolicyInjectionBehavior&gt;(),
  new Interceptor&lt;TransparentProxyInterceptor&gt;());
```

You can then use one of the attributes, such as the **LogCallHandler** attribute, defined in the Enterprise Library blocks as shown in the following example.  

```csharp
[LogCallHandler(AfterMessage = "After call",
                BeforeMessage = "Before call",
                Categories = new string[] { "General" },
                EventId = 9002,
                IncludeCallStack = false,
                IncludeCallTime = true,
                IncludeParameters = true,
                LogAfterCall = true,
                LogBeforeCall = true,
                Priority = 10,
                Severity = System.Diagnostics.TraceEventType.Information,
                Order = 1)]
[CachingCallHandler(2)]
public Tenant GetTenant(string tenant)
{
  ...
}
```

You can customize the information contained in the log message using attribute parameters, and control the order of the handlers in the pipeline using the **Order** parameter.  
An alternative approach to configuring the Enterprise Library call handlers is through the **PolicyInjection** static façade. The sample code in the PIABSamples solution that accompanies this guide includes an example of this approach.  


## A Real World Example ##
This example is from the aExpense reference implementation that accompanies the Enterprise Library guidance. To install the sample, download the [Enterprise Library Reference Implementation](http://go.microsoft.com/fwlink/p/?LinkID=290917). The aExpense application is an ASP.NET application that makes extensive use of the Enterprise Library blocks, including Unity and Policy Injection. This example is taken from the aExpense implementation that use Enterprise Library 6.  
The goal is to use the Semantic Logging Application Block to log calls to a specific method in the data access layer of the application. The following code sample shows part of the **ExpenseRepository** class where the **SaveExpense** method has a **Tag** attribute that identifies a policy.  

```csharp
public class ExpenseRepository : IExpenseRepository
{
  ...
  
  [Tag("SaveExpensePolicyRule")]
  public virtual void SaveExpense(Model.Expense expense)
  {
    ...
  }

  ...
}
```

Note that the **ExpenseRepository** model class implements the **IExpenseRepository** interface: this means that you can use the **VirtualMethodInterceptor** interceptor type. The following code sample shows the registration of the **ExpenseRepository** type that includes adding the **PolicyInjectionBehavior**.  

```csharp
container
  .RegisterType&lt;IExpenseRepository, ExpenseRepository&gt;(
    new Interceptor&lt;VirtualMethodInterceptor&gt;(),
    new InterceptionBehavior&lt;PolicyInjectionBehavior&gt;());
```

The following code sample shows the **SemanticLogCallHandler** call handler class that performs the logging.  

```csharp
public class SemanticLogCallHandler : ICallHandler
{
    public SemanticLogCallHandler(NameValueCollection attributes)
    {
    }

    public IMethodReturn Invoke(IMethodInvocation input,
                                GetNextHandlerDelegate getNext)
    {
        if (getNext == null) throw new ArgumentNullException("getNext");

        AExpenseEvents.Log.LogCallHandlerPreInvoke(
          input.MethodBase.DeclaringType.FullName, input.MethodBase.Name);

        var sw = Stopwatch.StartNew();

        IMethodReturn result = getNext()(input, getNext);

        AExpenseEvents.Log.LogCallHandlerPostInvoke(
          input.MethodBase.DeclaringType.FullName, input.MethodBase.Name, 
          sw.ElapsedMilliseconds);

        return result;
    }

    public int Order { get; set; }
}
```

This call handler uses the Semantic Logging Application Block to log events before and after the call, and to record how long the call took to complete.  
The application configures the Policy Injection Application Block declaratively. The following snippet from the Web.config file shows how the **Tag** attribute is associated with the **SemanticLogCallHandler** type.  

```other
&lt;policyInjection&gt;
  &lt;policies&gt;
    &lt;add name="ExpenseTracingPolicy"&gt;
      &lt;matchingRules&gt;
        &lt;add name="TagRule" match="SaveExpensePolicyRule" ignoreCase="false" 
           type="Microsoft.Practices.EnterpriseLibrary.PolicyInjection
                 .MatchingRules.TagAttributeMatchingRule, ..."/&gt;
      &lt;/matchingRules&gt;
      &lt;handlers&gt;
        &lt;add type="AExpense.Instrumentation.SemanticLogCallHandler, AExpense,
                   Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" 
                   name="SemanticLogging Call Handler" /&gt;
      &lt;/handlers&gt;
    &lt;/add&gt;
  &lt;/policies&gt;
&lt;/policyInjection&gt;

```



# Summary #
In this chapter, you learned how to use Unity and the Unity container to add support for crosscutting concerns in your application by using interception. Interception enables you to implement support for crosscutting concerns while continuing to write code that follows the single responsibility principle and the open/closed principle. The chapter also described how you can use the policy injection technique that allows you define policies to manage how and where in your application you address the crosscutting concerns. As an alternative to defining policies that are typically managed in a single class in your project or in a configuration file, you can also use attributes that enable developers to decorate their code to specify how they want to address the crosscutting concerns.  
