---
Source File Name: 75-Interception.docx
AssetID: 29f44713-c61a-4b7c-a21e-9cd4d183ace3
Title: Stand-alone Unity Interception
Order In ToC: 2\5\1
Output Filename: 2\5\1_Stand-alone Unity Interception.markdown
---

#### Markdown Test ####
# Stand-alone Unity Interception #
----------


> ![(../../images/note.gif)#!155CharTopicSummary!#:
> <a name="interception_standalone" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>
You can use Unity interception as a stand-alone feature with no dependency injection container by using the Intercept class.

You can use Unity interception as a stand-alone feature with no dependency injection container by using the **Intercept** class. As with a container, interception as a stand-alone feature enables you to perform instance or type interception. The **Intercept** class contains the **NewInstance,** **NewInstanceWithAdditionalInterfaces,** **ThroughProxy,** and **ThroughProxyWithAdditionalInterfaces** methods, enabling you to perform either proxy or instance interception. And both methods include the **AdditionalInterfaces** parameter, enabling you to implement additional interfaces on the target object. This corresponds to the **AdditionalInterface** feature when using interception with a container.   

> ![(../../images/note.gif)Note:
> The first parameter on **Intercept.ThroughProxy, Intercept.ThroughProxyWithAdditionalInterfaces** and **Intercept. NewInstance** is the corresponding interceptor when setting up interception through the stand-alone API.

The **AdditionalInterfaces** parameter on the object enables you to receive more messages and to augment the set of methods the object can respond to.   
This section contains the following sections describing stand-alone interception:  
+ <a href="#Standalone_proxy" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Stand-Alone Interception with a Proxy</a> describes creating a proxy to the intercepted instance.
+ <a href="#Standalone_type" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Stand-Alone Interception with a Derived Type</a> describes interception by using a derived type.

# Stand-Alone Interception with a Proxy #
<a name="Standalone_proxy" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Instance interceptors work by creating a proxy to the intercepted instance. The following examples create a Proxy interface interceptor. The first example implements no additional interfaces.   

```csharp
IInterface proxy = Intercept.ThroughProxy&lt;IInterface>(
        new BaseClass(10),
        new InterfaceInterceptor(),
        new[] { interceptionBehavior }
);
```


```vb
Dim proxy As IInterface = Intercept.ThroughProxy(Of IInterface)( _
        New BaseClass(10), _
        New InterfaceInterceptor(), _
        New IInterceptionBehavior() {interceptionBehavior} _
)
```

The following example uses the last parameter to specify additional interfaces to implement. You can specify as many interfaces as you like.   

```csharp
IInterface proxy = (IInterface)Intercept.ThroughProxyWithAdditionalInterfaces(
        typeof(IInterface),
        new BaseClass(10),
        new InterfaceInterceptor(),
        new[] { interceptionBehavior },
        new[] { typeof(ISomeInterface), typeof(IsomeOtherInterface) });
```


```vb
Dim proxy As IInterface = DirectCast(Intercept.ThroughProxyWithAdditionalInterfaces( _
        GetType(IInterface), _
        New BaseClass(10), _
        New InterfaceInterceptor(), _
        New IInterceptionBehavior() { interceptionBehavior }, _
        New Type() { GetType(ISomeInterface), GetType(IsomeOtherInterface) })
```


# Stand-alone Interception with a Derived Type #
<a name="Standalone_type" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>You can use **Intercept.NewInstance** to set up type interception using a derived type. You can use the **additionalInterfaces** parameter to implement additional interfaces on the target object. You can provide any number of interfaces for this parameter. Unity provides one derived type interceptor: the virtual method interceptor. The last parameter contains arguments for the creation of the new instance and is an arbitrary number in this example.  

```csharp
BaseClass instance =
    Intercept.NewInstance&lt;BaseClass>(
        new VirtualMethodInterceptor(),
        new[] { interceptionBehavior },
        10);
```


```vb
Dim instance As BaseClass = _
    Intercept.NewInstance(Of BaseClass) _
        (New VirtualMethodInterceptor(), _
        New IInterceptionBehavior() {interceptionBehavior}, _
        10)
```

This is the same as the previous example, except additional interfaces are specified.  

```csharp
BaseClass instance =
    Intercept.NewInstanceWithAdditionalInterfaces&lt;BaseClass>(
        new VirtualMethodInterceptor(),
        new[] { interceptionBehavior },
        new[] { typeof(ISomeInterface), typeof(IsomeOtherInterface)},
        10);
```


```vb
Dim instance As BaseClass = _
    Intercept.NewInstanceWithAdditionalInterfaces(Of BaseClass)( _
        New VirtualMethodInterceptor(), _
        New IInterceptionBehavior() {interceptionBehavior}, _
        New Type(){GetType(ISomeInterface), GetType(IsomeOtherInterface)}, _
        10)
```



