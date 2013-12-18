---
Source File Name: Unity Instructions CS template.docx
AssetID: 8b917b64-61d5-4a81-a1c8-9d660465e113
Title: Lab 6: Integrating with ASP.NET MVC and Registration by Convention
Order In ToC: 4-6
Output Filename: 4-6_Lab 6- Integrating with ASP.NET MVC and Registration by Convention.markdown
---

#### Markdown Test ####
# Lab 6: Integrating with ASP.NET MVC and Registration by Convention #
----------

Estimated time to complete this lab: **20 minutes**  


## Introduction ##
In this lab, you will practice using Unity with an ASP.NET MVC application and taking advantage of registration by convention to register several types using a single instruction.   
To begin, open the StocksTicker.sln file located in the Lab06\begin\StocksTicker folder.  


### Reviewing the Application ###
The application is an ASP.NET MVC port of the application from the previous lab. The **StocksTickerController** class is the one that handles the user’s input and requests the quotes from the **IStockQuoteService**.  


## Task 1: Integrating with ASP.NET MVC ##
In this task, the application will be updated to use a UnityContainer to resolve the dependencies in an ASP.NET MVC application.  


### Adding References to the Required Assemblies ###
**To add references to the required assemblies**

1. Add a reference to the **Unity bootstrapper for ASP.NET MVC** NuGet package.![](images/note.gif)Note:&gt; This package will add source code to your project in addition to references to its binaries and the **Unity** NuGet package it depends on. The added files are **UnityConfig.cs** and **UnityMvcActivator.cs** in the **App_Start** folder.



### Updating the StocksTickerController to Receive its Dependencies in the Constructor ###
**To update the StocksTickerController**

1. Open the StocksTickerController.cs file.
2. Update the constructor to receive the stock quote service as a parameter rather than creating it, as shown in the following highlighted code.
```other
public StocksTickerController(<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">IStockQuoteService service</span></b>)
{
this.service = <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">service</span>;
} 
```




### Updating the Unity Container Configuration Code ###
The **UnityConfig.cs** file is intended to be updated with the desired container configuration for the application, either loading it from a configuration file or specifying it programmatically. In this case the container will be configured programmatically.  
**To Update the Container Configuration**

1. Open the **UnityConfig.cs** file located in the **App_Start** folder.
2. Add **using** statements for the required namespaces:
```other
using StocksTicker.Loggers;
using StocksTicker.StockQuoteServices; 
```


3. Add the following code to the **RegisterTypes** method as shown in the following highlighted code. These registrations are equivalent to those specified in the previous lab using the configuration file.
```other
public static void RegisterTypes(IUnityContainer container)
{
// NOTE: To load from web.config uncomment the line below. Make sure to add a Microsoft.Practices.Unity.Configuration to the using statements.
// container.LoadConfiguration();

// TODO: Register your types here
// container.RegisterType&lt;IProductRepository, ProductRepository&gt;();

<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">container.RegisterType&lt;ILogger, TraceSourceLogger&gt;(</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new ContainerControlledLifetimeManager(),</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new InjectionConstructor("default"));</span></b>

<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">container</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    .RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;(</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    new ContainerControlledLifetimeManager(),</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    new InjectionProperty("Logger"));</span></b>
}  
```




### Updating the Unity Container Activation Code ###
The **UnityMvcActivator.cs** file contains the code to hook up the Unity container to the various ASP.NET MVC dependency resolution mechanisms when the application starts. The following updates to this file will ensure the container is also disposed when the application shuts down.  
**To Update the Container Activation**

1. Open the **UnityMvcActivator.cs** file.
2. Add a shutdown activation attribute for the assembly as shown in the following highlighted code.
```other
[assembly: WebActivatorEx.PreApplicationStartMethod(
typeof(StocksTicker.App_Start.UnityWebActivator), "Start")]
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">[assembly: WebActivatorEx.ApplicationShutdownMethod(</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">typeof(StocksTicker.App_Start.UnityWebActivator), "Shutdown")]</span></b>

namespace StocksTicker.App_Start 
```


3. Add the new **Shutdown** method to the **UnityWebActivator** class to dispose the container.
```other
public static void Shutdown()
{
var container = UnityConfig.GetConfiguredContainer();
container.Dispose();
}  
```




### Running the Application ###
**To run the application**

1. Launch the application. Use the application for a while, subscribing to a few symbols. ![](images/85A69AB042A17C3CBBE620D5B843B1BC.png)  

2. Close the browser and stop the development server. To do this, right-click the IIS Express icon in the notification area of the taskbar and click **Exit**.
3. Open the **trace.log** file located in the **StocksTicker** folder (the project’s folder in the file system). The contents at the bottom of the file should look like the following log, with messages indicating the processing of the different sets of requests and the final two entries indicating that the service and the logger were disposed.
```other
default Information: 0 : Generating random quotes
    DateTime=2013-09-10T16:35:18.3929349Z
default Information: 0 : Generating random quotes
    DateTime=2013-09-10T16:35:43.1794342Z
default Information: 0 : Generating random quotes
    DateTime=2013-09-10T16:35:48.5441286Z
default Information: 0 : Generating random quotes
    DateTime=2013-09-10T16:36:13.3243585Z
default Information: 0 : Generating random quotes
    DateTime=2013-09-10T16:36:18.6529855Z
default Information: 0 : Generating random quotes
    DateTime=2013-09-10T16:36:43.4656698Z
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">default Information: 0 : Shutting down service</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    DateTime=2013-09-10T16:36:46.9271699Z</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">default Information: 0 : Shutting down logger</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    DateTime=2013-09-10T16:36:46.9301664Z  </span></b>
```




## Task 2: Using Registration by Convention ##
In this task, the application will be updated to register types using registration by convention.   
When using registration by convention, several types that require similar registrations are registered simultaneously rather than on a per-type basis. This greatly simplifies the configuration code, but it does require these types to require as little type-specific configuration as possible.  
Registration by convention is only available when configuring a container programmatically.  


### Updating the RandomStockQuoteService to Receive its Dependencies in the Constructor ###
Unity will perform constructor injection automatically, but property injection must be explicitly specified by annotating the property or adding property injection instructions when configuring the container. Switching from property to constructor injection avoids the configuration specific for the type and the Unity-specific attributes in the target type.  
**To update the RandomStockQuoteService**

1. Open the **RandomStockQuoteService.cs** file.
2. Update the constructor to receive the logger as a parameter rather than creating it, as shown in the following highlighted code.
```other
public RandomStockQuoteService(<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">ILogger logger</span></b>)
{
    this.logger = <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">logger</span></b>;
} 
```




### Updating the Unity Container Configuration Code ###
Using registration by convention is similar to type-specific container configuration, and uses similarly structured methods.   
**To Update the Container Configuration**

1. Open the **UnityConfig.cs** file located in the **App_Start **folder.
2. Add these **using** directives.
```other
using System.Linq;
using System.Web.Mvc;
```


3. Add a **RegisterTypes** call to perform registration by convention in the **RegisterTypes** method:
```other
public static void RegisterTypes(IUnityContainer container)
{
    // NOTE: To load from web.config uncomment the line below. Make sure to add a Microsoft.Practices.Unity.Configuration to the using statements.
    // container.LoadConfiguration();

    // TODO: Register your types here
    // container.RegisterType&lt;IProductRepository, ProductRepository&gt;();

<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">container.RegisterTypes(</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">        AllClasses.FromAssemblies(typeof(UnityConfig).Assembly)</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    .Where(t =&gt; !(t.IsSubclassOf(typeof(Controller)))),</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">        WithMappings.FromAllInterfacesInSameAssembly,</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">        WithName.Default,</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">        WithLifetime.ContainerControlled);</span></b>

    container.RegisterType&lt;ILogger, TraceSourceLogger&gt;(
        new ContainerControlledLifetimeManager(),
        new InjectionConstructor("default"));

    container.RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;(
        new ContainerControlledLifetimeManager(),
        new InjectionProperty("Logger"));
} 
```


4. The first parameter in call to **RegisterTypes** indicates the types involved in the registration by convention. In this case the types are all those types in the application’s assembly which are not Controller implementations. Note how the utility method **AllClasses.FromAssemblies** is used to get all the types and a LINQ query is use to filter out the unwanted types.
5. The second parameter indicates which other types (usually interfaces) should be mapped to each type to register. In this case, types will be mapped to all interfaces that belong to the same assembly as the registered types.
6. The third parameter indicates the name to use when registering each type. In this case, the default name will be used. This allows the Unity container to use these registrations to inject dependencies without the need for explicit configuration.The fourth parameter indicates the lifetime manager to use for each type. In this case, a container controlled lifetime manager will be used for each type.  
![](images/note.gif)Note:&gt; All parameters except the enumeration of types to register is a delegate which indicates how each type should be registered according to the convention. Unity provides methods in the **WithMappings**, **WithName** and **WithLifetime** classes which address generally useful conventions, but custom methods or in-line lambda expressions can be used instead. 
7. Remove the registration for the **RandomStockQuoteService **class:
```other
public static void RegisterTypes(IUnityContainer container)
{
    // NOTE: To load from web.config uncomment the line below. Make sure to add a Microsoft.Practices.Unity.Configuration to the using statements.
    // container.LoadConfiguration();

    // TODO: Register your types here
    // container.RegisterType&lt;IProductRepository, ProductRepository&gt;();

    container.RegisterTypes(
        AllClasses.FromAssemblies(typeof(UnityConfig).Assembly)
    .Where(t =&gt; !(t.IsSubclassOf(typeof(Controller)))),
        WithMappings.FromAllInterfacesInSameAssembly,
        WithName.Default,
        WithLifetime.ContainerControlled);

    container.RegisterType&lt;ILogger, TraceSourceLogger&gt;(
        new ContainerControlledLifetimeManager(),
        new InjectionConstructor("default"));

<b><s>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">container.RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;(</span></s></b>
<b><s><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">        new ContainerControlledLifetimeManager(),</span></s></b>
<b><s><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">        new InjectionProperty("Logger"));</span></s></b>
} 
```


8. This registration is no longer needed since the type does not require property injection and the registration by convention instruction takes care of the type mapping and the lifetime management.
9. 

10. Remove the unnecessary parts of the registration for the **TraceSourceLogger** class:
```other
public static void RegisterTypes(IUnityContainer container)
{
    // NOTE: To load from web.config uncomment the line below. Make sure to add a Microsoft.Practices.Unity.Configuration to the using statements.
    // container.LoadConfiguration();

    // TODO: Register your types here
    // container.RegisterType&lt;IProductRepository, ProductRepository&gt;();

    container.RegisterTypes(
        AllClasses.FromAssemblies(typeof(UnityConfig).Assembly)
    .Where(t =&gt; !(t.IsSubclassOf(typeof(Controller)))),
        WithMappings.FromAllInterfacesInSameAssembly,
        WithName.Default,
        WithLifetime.ContainerControlled);

    container.RegisterType&lt;<b><s><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">ILogger,</span></s></b> TraceSourceLogger&gt;(
        <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new ContainerControlledLifetimeManager(),</span>
        new InjectionConstructor("default"));
} 
```


The **TraceSourceLogger** type still needs explicit configuration for the constructor injection. It is allowed to use standard registration and registration by convention for the same type, and the usual precedence rules apply and the last registration is used.  
![](images/note.gif)Note:&gt; In this example the registration by convention involves more code than the standard registration of the required types. The more types involved, the more useful registration by convention becomes.

### Removing the NullLogger class to avoid Overlapping Mappings ###
When registering multiple types there’s the risk of register mappings to the same interface several times. In the case of the Unity container, this results in the last mapping registered taking precedence, which may not be the desired outcome. There are several ways to prevent this overlap, which require evaluating their trade-offs. The approach taken in this lab is to avoid the problem by having no two classes implementing the same interface. Alternatively, the registration by convention instruction could be issues so that only “matching interfaces” are mapped, which requires the names of the interface and the class to be the same except for a leading ‘I’ in the interface’s name. Finally, registration by convention could assign different names to each registration, usually using the class name as the registration name, but this would require all dependencies to the interface to be explicitly configured with a name, greatly complicating the configuration and defeating the purpose of registration by convention.  

**To remove the NullLogger class**

1. In Solution Explorer, select the **NullLogger.cs** file (located under Loggers solution folder).
2. Click **Delete** on the **Edit **menu. 


### Running the Application ###
Launch and use the application as in the previous task. It will have the same behavior, as only the way the mappings are configured has changed.  
To verify you have completed the lab correctly, you can use the solution provided in the Lab06\end\StocksTicker folder.  


# More Information #
For more information about Unity, see the documentation in the [Unity 3 Reference documentation](http://go.microsoft.com/fwlink/p/?LinkID=317483) and the [Developer's Guide to Dependency Injection Using Unity](http://go.microsoft.com/fwlink/p/?LinkID=290920).  

