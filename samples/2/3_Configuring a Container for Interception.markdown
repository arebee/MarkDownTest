---
Source File Name: 75-Interception.docx
AssetID: 6a974ef0-4f5e-407f-b196-b126a08f9205
Title: Configuring a Container for Interception
Order In ToC: 2\3
Output Filename: 2\3_Configuring a Container for Interception.markdown
---

#### Markdown Test ####
# Configuring a Container for Interception #
----------


> ![](../images/note.gif)#!155CharTopicSummary!#:
> 
If you choose to use the Unity DI container, you can configure the container by using a configuration file or at run time by using the API.

You can use interception with or without a dependency injection (DI) container such as Unity. Using a DI container relieves you of the need to manually create all the dependencies and pass them into the correct objects. If you choose to use the Unity DI container, you can configure the container by using a configuration file or at run time by using the API.   
In Unity, interception is just another extension point instead of a self-contained part of the configuration file, as in previous versions. Prior to Unity 2.0 when you specified interception, policy injection was implicitly configured by the underlying code. Where the behavior of interception was implicit before, it is explicit now; you specify that interception is to happen and you specify what is to happen upon interception. Interception becomes just one more thing that you can describe about how an object is resolved. Unity version 3 enables you to modify how interception happens and how the object is created.  
There are two approaches for setting up Unity interception:  
+ The approach introduced in Unity 2.0 in which interception is configured as just another extension point element in the entry for a container type. Configure an extension point by using the **<sectionExtension>** and **<extension>** tags in the configuration file or by using the **RegisterType** method at run time. 
+ The pre-Unity 2.0 approach used in earlier versions of Unity in which interception is a self-contained part of the container configured by using the interception extension elements, <**extensionConfig**> and <**interceptors**> in the configuration file or by using the **SetInterceptorFor **or **SetDefaultInterceptorFor** methods on the interception container extension. This earlier style is still available primarily for backward compatibility.
You can use a configuration file to specify a container-created behavior with any logical configuration. For detailed interception schema information, see [Configuration Files for Interception](http://msdn.microsoft.com/library/af2f3726-4a3e-4e31-8f97-ebca0db3d907).  
This topic contains the following sections to describe how to configure a container for interception:  
+ <a href="#interception_config_ext" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Adding the Interception Extension to the Container</a>
+ <a href="#interception_config_type" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Configuring Interception of a Type</a>
+ <a href="#interception_config_moreinfo" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">More Information</a>

# Adding the Interception Extension to the Container #
<a name="interception_config_ext" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>You can add interception support to the Unity container through API calls or through a configuration file. This is true for all versions.   
The following example uses the API to add interception to the Unity container by calling **AddNewExtension<Interception>** on the container.  

```csharp
IUnityContainer container = new UnityContainer();
container.AddNewExtension<Interception>();
```


```vb
Dim container As IUnityContainer = New UnityContainer()
container.AddNewExtension(Of Interception)()
```

The following example adds an interception extension by using the configuration file. It adds interception by including a **sectionExtension** element that points to the default built-in interception extension.  

```other
<sectionExtension type=
    "Microsoft.Practices.Unity.InterceptionExtension.Configuration.
             InterceptionConfigurationExtension,
     Microsoft.Practices.Unity.Interception.Configuration" />
```

The **sectionExtension** **type** provides a context object, which can add aliases, container configuration elements, injection member elements, and value elements. You can then use these aliases and elements to configure containers for interception. The following example uses the element added by the default interception extension to specify a **TransparentProxyInterceptor** interceptor for a specific build key or for all resolved instances of a type.  

```other
    <container name="YourContainer">
      <extension type="Interception" />
    </container>
```

When you add the **sectionExtension** element for the interception configuration extension, new aliases and element names related to interception become available. In the case of the interception extension, the **Interception** name is aliased to the full name of the **Microsoft.Practices.Unity.InterceptionExtension.Interception** class, and the **<interceptor>** element becomes available. In Unity 1.2, where there was no **sectionExtension** element, you had to add the element using the full type name, as in the following extract:  

```other
 <add elementType="the full name for the interceptor configuration element type" type="VirtualMethodInterceptor"/>
```


> ![](../images/note.gif)Note:
> For all custom extensions, you must use the full name for the extension when adding it with the <**extension**> element, unless you specify an alias in the **sectionExtension** context object.


# Configuring Interception of a Type #
<a name="interception_config_type" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Once you have added interception to the container, you can then configure the container to intercept calls on a type and configure the behaviors for that interception. There are two approaches to configuring interception of a type, both configurable by using the API or by using a configuration file:  
+ Use the Unity 2.0 approach, explicitly configuring interceptor, behaviors, and additional interfaces. 
+ Use the pre-Unity 2.0 approach, configuring interception as a self-contained part of the configuration file using the interception extension elements, <**extensionConfig**> and <**interceptors**>.

```other
      <register type="Interceptable">
        <interceptor type="VirtualMethodInterceptor" />
      </register>    
```

For information on configuring interception at design time see [Configuration Files for Interception](http://msdn.microsoft.com/library/af2f3726-4a3e-4e31-8f97-ebca0db3d907) and for run-time information see [Registering Interception](http://msdn.microsoft.com/library/53570dcb-4520-4e42-b64d-84c9222841c0).  


# Unity 2.0 Approach #
The Unity 2.0 approach explicitly configures the interceptor, behaviors, and additional interfaces. You can configure the interceptor, behaviors, and additional interfaces using the same mechanism and elements you use to configure injection through constructors and properties.  

> ![](../images/note.gif)Note:
> When performing interception using the Unity 2.0 **RegisterType** API you can target a specific key or instance and you can target all keys or instances of a type. 
At run time, target a specific key or instance by using the **Interceptor** injection member. Use the **DefaultInterceptor** injection member to target all keys or instances of a type.
At design time, set the **DefaultForType** attribute in the **<interceptor>** element to target all keys or instances of a type. 
Likewise, you can specify that behaviors should apply to all instances of a type in the container by using the **DefaultInterceptorBehavior** injection member. 

<a name="_Registering_Interceptors_Using" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following example configures the container at run time by including **Interceptor**, **InterceptionBehavior,** and **AdditionalInterface** calls to **RegisterType**() on the container where **IOtherInterface** is the interface to add. In this extract, myInterceptor is the name used to register the **TypeToIntercept**.   

```csharp
IUnityContainer container = new UnityContainer();
container.AddNewExtension<Interception>();
container.RegisterType<TypeToIntercept>(
        "myInterceptor",
        new Interceptor<InterfaceInterceptor>(),
        new AdditionalInterface(typeof(IOtherInterface)),
        new InterceptionBehavior<CustomBehavior>("someBehavior"));
```


```vb
Dim container As IUnityContainer = New UnityContainer()
container.AddNewExtension(Of Interception)()
container.RegisterType(Of typetointercept)( _
        "myInterceptor", _
        New Interceptor(Of InterfaceInterceptor)("interceptorName"), _
        New AdditionalInterface(GetType(IOtherInterface)), _
        New InterceptionBehavior(Of CustomBehavior)("someBehavior"))
```

The following example configures the container **Interception** extension by adding the appropriate **InterceptorElement**, **InterceptionBehaviorElement,** and **AddInterfaceElement** elements in the configuration file; in this example **interceptor**, **interceptionBehavior,** and **addInterface**, which are all elements defined in the extension from **sectionExtension**.   

```other
<container name="YourContainer">
  <extension type="Interception" />
  <register type="TypeToIntercept">
    <interceptor type="VirtualMethodInterceptor" />
    <addInterface type="IServiceProvider" />
    <addInterface type="IComponent" />
    <interceptionBehavior type="CustomBehavior" name="someBehavior" />
  </register>
</container>
```



# Legacy Approach #
The pre-Unity 2.0 approach to configuring the container at run time called the **Configure** and the **SetInterceptorFor** (or **SetDefaultInterceptorFor**) methods on the interception extension. For details on legacy API see "Registering Interceptors Using SetInterceptorFor" in the [Registering Interception](http://msdn.microsoft.com/library/53570dcb-4520-4e42-b64d-84c9222841c0) topic.  

> ![](../images/note.gif)Note:
> The pre-Unity 2.0 approach automatically adds the policy injection. The behavior is implicitly set up in the underlying code. 


# More Information #
<a name="interception_config_moreinfo" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>For information on configuring the Unity DI container, see the following Unity topics:  
+ [Configuration Files for Interception](http://msdn.microsoft.com/library/af2f3726-4a3e-4e31-8f97-ebca0db3d907)
+ [Registering Interception](http://msdn.microsoft.com/library/53570dcb-4520-4e42-b64d-84c9222841c0)


