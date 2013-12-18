---
Source File Name: Unity Instructions CS template.docx
AssetID: a2e901a6-acf1-47e2-98c2-a677377ea2f3
Title: Lab 5: Integrating with ASP.NET and Child Containers
Order In ToC: 4-5
Output Filename: 4-5_Lab 5- Integrating with ASP.NET and Child Containers.markdown
---

#### Markdown Test ####
# Lab 5: Integrating with ASP.NET and Child Containers #
----------

Estimated time to complete this lab: **20 minutes**  


## Introduction ##
In this lab, you will practice using Unity with an ASP.NET application. Because the creation of ASP.NET objects is managed by the ASP.NET infrastructure, the Unity container cannot be used to create them. Instead, the container will be used to apply injection to existing objects using the **BuildUp** method. For information about the **BuildUp** methods, see the topic "[Using BuildUp to Wire Up Objects Not Created by the Container](http://go.microsoft.com/fwlink/p/?LinkID=317512)" in the Unity 3 documentation.   
To begin, open the StocksTicker.sln file located in the Lab05\begin\StocksTicker folder.  


### Reviewing the Application ###
The application is a simplified ASP.NET version of the stocks ticker application used throughout these labs. Just like with the Windows Forms version of the application, stock symbols can be subscribed to, and the latest updates on the stocks are displayed in a table, as illustrated in the following figure.  
![](images/5a914359-e835-4f2b-8fec-edc818ef58b9.png)  
<a name="_Toc223172426" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Task 1: Integrating with ASP.NET ##
In this task, the application will be updated to use a single, application-wide container to perform injection on its Web forms before they process requests.  


### Adding References to the Required Assemblies ###
**To add references to the required assemblies**

1. Add a reference to the **Unity** NuGet package.
2. Add a reference to the .NET Framework's **System.Configuration** assembly.


### Updating the Default Page to Use Property Injection ###
**To update the default page to use property injection**

1. Open the Default.aspx.cs file. If it is not visible, expand the node for the Default.aspx file.
2. Add a **using** directive for the **Unity** namespace.
```other
using Microsoft.Practices.Unity;
```


3. Remove the initialization code from the **Page_Load** method, as shown in the following highlighted code.
```other
protected void Page_Load(object sender, EventArgs e)
{
<b>  <s><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">this.stockQuoteService = new RandomStockQuoteService();</span></s></b>
  if (!this.IsPostBack)
  {
    UpdateQuotes();
  } 
}
```


4. Add the **Dependency** attribute to the **StockQuoteService**, as shown in the following highlighted code.
```other
private IStockQuoteService stockQuoteService;
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">[Dependency]</span></b>
public IStockQuoteService StockQuoteService
{
  get { return stockQuoteService; }
  set { stockQuoteService = value; }
}
```

![](images/note.gif)Note:&gt; Configuration files cannot be used in this case because the assembly name for the page class is not known in advance so assembly-qualified type names cannot be expressed.



### Adding a Global.asax File to Manage the Container ###
The **HttpApplication** class is used to define common methods in an ASP.NET application, so the code to manage the Unity container will be added to a new Global.asax** **file. For information about HTTP application classes, see [HttpApplication Class](http://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx).   
**To add a Global.asax file to manage the container**

1. Add a Global.asax file. Right-click the **StocksTicker** project node, point to **Add**, and then click **New Item**. Select the **Global Application Class** template, and then click **Add**.![](images/59a8d57c-ffff-4e3f-9a9d-a0ec6069665a.png)  

2. Add **using** statements for the required namespaces:
```other
using Microsoft.Practices.Unity;
using Microsoft.Practices.Unity.Configuration;
using System.Web.Configuration;
using System.Web.UI;
```


3. Add a constant named **AppContainerKey** with **"application container"** as its value as shown in the following highlighted code.
```other
public class Global : System.Web.HttpApplication
{
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">private const string AppContainerKey = "application container";</span></b>

  protected void Application_Start(object sender, EventArgs e)
  {
    ...
  }
```


4. Add the following code to implement an **ApplicationContainer** property to store a Unity container in the application state using the key defined earlier.
```other
private IUnityContainer ApplicationContainer
{
  get
  {
    return (IUnityContainer)this.Application[AppContainerKey];
  }
  set
  {
    this.Application[AppContainerKey] = value;
  }
}
```

![](images/note.gif)Note:&gt; The container is stored in the application's shared state. 
5. Update the empty **Application_Start** method to create a Unity container, configure it with the information for the **"application"** container to be defined in the configuration file and set it as the value for the **ApplicationContainer** property as shown in the following highlighted code.
```other
protected void Application_Start(object sender, EventArgs e)
{
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">IUnityContainer applicationContainer = new UnityContainer();</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">applicationContainer.LoadConfiguration("application");</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">ApplicationContainer = applicationContainer;</span></b>
}
```


6. Update the empty **Application_End** method to dispose the application's container as shown in the following highlighted code.
```other
protected void Application_End(object sender, EventArgs e)
{
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">IUnityContainer applicationContainer = this.ApplicationContainer;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">if (applicationContainer != null)</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">{</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">applicationContainer.Dispose();</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">this.ApplicationContainer = null;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">}</span></b>
}
```


7. Add the following code to implement a method named **Application_PreRequestHandlerExecute** to intercept the ASP.NET execution pipeline and use the container to inject dependencies into the request handler if the handler is a **Page**.
```other
protected void Application_PreRequestHandlerExecute(
                                     object sender, EventArgs e)
{
  Page handler = HttpContext.Current.Handler as Page;

  if (handler != null)
  {
    IUnityContainer container = ApplicationContainer;

    if (container != null)
    {
      container.BuildUp(handler.GetType(), handler);
    }
  }
}
```




### Adding Unity Configuration to the Web.config File ###
The configuration in a Web.config file uses the same schema as in an App.config file.  
**To add Unity configuration to the Web.config file**

1. Open the Web.config file.
2. Add a declaration for the **unity** configuration section.
```other
&lt;configSections&gt;
  ...
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;section name="unity" type="Microsoft.Practices.Unity.Configuration.UnityConfigurationSection, Microsoft.Practices.Unity.Configuration"/&gt;</span></b>
&lt;/configSections&gt;
```


3. Add the **unity** section element.
```other
  &lt;/configSections&gt;
<b>  ...</b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;unity xmlns="http://schemas.microsoft.com/practices/2010/unity"&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/unity&gt;</span></b>
```


4. Add **alias** elements for the types used throughout the labs.
```other
&lt;unity xmlns="http://schemas.microsoft.com/practices/2010/unity"&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;alias alias="TraceSource" type="System.Diagnostics.TraceSource, System "/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;alias alias="ILogger" type="StocksTicker.Loggers.ILogger, StocksTicker"/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;alias alias="TraceSourceLogger"</span></b>
<b>         <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">type="StocksTicker.Loggers.TraceSourceLogger, StocksTicker"/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;alias alias="IStockQuoteService"</span></b>
<b>         <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">type="StocksTicker.StockQuoteServices.IStockQuoteService,</span></b>
<b>               <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">StocksTicker"/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;alias alias="RandomStockQuoteService"</span></b>
<b>         <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">type="StocksTicker.StockQuoteServices.RandomStockQuoteService,</span></b>
<b>               <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">StocksTicker"/&gt;</span></b>
&lt;/unity&gt;
```


5. Add a **container** element with the name **application**.
```other
  &lt;alias alias="RandomStockQuoteService"
         type="StocksTicker.StockQuoteServices.RandmoStockQuoteService,
               StocksTicker"/&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;container name="application"&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/container&gt;</span></b>
&lt;/unity&gt;
```


6. Add a **register** element to map the **IStockQuoteService** interface to the **RandomStockQuoteService** class with a **property** element to inject the **Logger** property and define a singleton lifetime manager.
```other
&lt;container name="application"&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register type="IStockQuoteService" mapTo="RandomStockQuoteService"&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;lifetime type="singleton"/&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;property name="Logger"/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/register&gt;</span></b>
&lt;/container&gt;
```


7. Add a **register** element to map the **ILogger** interface to the **TraceSourceLogger** class, defining a singleton lifetime manager and injecting the "**default"** string in its **traceSourceName** constructor parameter. This mapping is required to inject the **Logger** property on the **RandomStockQuoteService** instance.
```other
&lt;container name="application"&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register type="ILogger" mapTo="TraceSourceLogger"&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;lifetime type="singleton"/&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;constructor&gt;</span></b>
<b>      <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;param name="traceSourceName"&gt;</span></b>
<b>        <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;value value="default"/&gt;</span></b>
<b>      <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/param&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/constructor&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/register&gt;</span></b>
  &lt;register type="IStockQuoteService" mapTo="RandomStockQuoteService"&gt;
    &lt;lifetime type="singleton"/&gt;
    &lt;property name="Logger"/&gt;
  &lt;/register&gt;
&lt;/container&gt;
```




### Running the Application ###
**To run the application**

1. Launch the application. Open two browser instances, one of them set for private browsing, as shown in the following figure, and open the application's URL in them both. Use the application for a while in both browsers, subscribing to different sets of symbols.![](images/2c7bd3b4-e0dc-4b6a-9353-6b1e6e591844.png)  

2. Close the browsers and stop the development server. To do this, right-click the Visual Studio development Web server icon in the notification area of the taskbar and click **Stop**.
3. Open the trace.log file located in the StocksTicker folder. The contents at the bottom of the file should look like the following log, with messages indicating the processing of the different sets of requests and the final two entries indicating that the service and the logger were disposed.
```other
default Information: 0 : Generating random quotes
    DateTime=2013-08-21T16:29:19.1468695Z
default Information: 0 : Generating random quotes
    DateTime=2013-08-21T16:29:49.1150004Z
default Information: 0 : Generating random quotes
    DateTime=2013-08-21T16:30:19.2251892Z
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">default Information: 0 : Shutting down service</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    DateTime=2013-08-21T16:30:20.0489271Z</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">default Information: 0 : Shutting down logger</span></b>
<span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    DateTime=2013-08-21T16:30:20.0589271Z</span> 
```




## Task 2: Using Per-Session Child Containers ##
In this task, a more sophisticated container management strategy will be implemented: the application-wide container will hold services shared by all sessions, but other objects will be managed by a session-specific container. In this case, the service used to retrieve quotes will not be shared, but its lifetime will be managed. Of course, this approach works only when session state is enabled and stored InProc.  
Container hierarchies can be used to control the scope and lifetime of objects and to register different mappings in different context. In this task, a two-level hierarchy will be implemented. For information about container hierarchies, see "[Using Container Hierarchies](http://go.microsoft.com/fwlink/p/?LinkID=317513)" in the Unity 3 documentation.   


### Updating the Global.asax File to Manage Per-Session Containers ###
**To update the Global.asax file to manage per-session containers**

1. Open the Global.asax.cs file.
2. Add a constant named **SessionContainerKey** with **"session container" **as its value as shown in the following highlighted code.
```other
public class Global : System.Web.HttpApplication
{
  private const string AppContainerKey = "application container";
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">private const string SessionContainerKey = "session container";</span></b>
```


3. Add the following code to implement a **SessionContainer** property to store a Unity container in the application state using the key defined earlier.
```other
private IUnityContainer SessionContainer
{
  get
  {
    return (IUnityContainer)this.Session[SessionContainerKey];
  }
  set
  {
    this.Session[SessionContainerKey] = value;
  }
}
```


4. Add the following highlighted code to the **Session_Start** method to create a child container of the application's container using the **CreateChildContainer** method, configure it with the information for the **"session"** container from the configuration file and set it as the value for the **SessionContainer** property.
```other
protected void Session_Start(object sender, EventArgs e)
{
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">IUnityContainer applicationContainer = this.ApplicationContainer;</span></b>

<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">if (applicationContainer != null)</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">{</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">IUnityContainer sessionContainer</span></b>
<b>            <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">= applicationContainer.CreateChildContainer();</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">sessionContainer.LoadConfiguration("session");</span></b>

<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">this.SessionContainer = sessionContainer;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">}</span></b>
}
```


5. Add the following highlighted code to the **Session_End** method to dispose the session's container.
```other
protected void Session_End(object sender, EventArgs e)
{
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">IUnityContainer sessionContainer = this.SessionContainer;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">if (sessionContainer != null)</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">{</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">sessionContainer.Dispose();</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">this.SessionContainer = null;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">}</span></b>
}
```


6. Update the **Application_PreRequestHandlerExecute** method to use the session container to **BuildUp** the request's handler as shown in the following highlighted code.
```other
protected void Application_PreRequestHandlerExecute(
                                     object sender, EventArgs e)
{
  Page handler = HttpContext.Current.Handler as Page;

  if (handler != null)
  {
    IUnityContainer container = <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">SessionContainer</span></b>;

    if (container != null)
    {
      container.BuildUp(handler.GetType(), handler);
    }
  }
}
```




### Updating the Unity Configuration with a Session Container ###
**To update the Unity configuration with a session container**

1. Open the Web.config file.
2. Add a new **container** element with name **session**.
```other
  &lt;/container&gt;
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;container name="session"&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/container&gt;</span></b>
&lt;/unity&gt;
```


3. Add a **register** element to map the **IStockQuoteService** interface to the **MoneyCentralStockQuoteService** class with a **property** element to inject the **Logger** property and define a singleton lifetime manager.
```other
&lt;container name="session"&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register type="IStockQuoteService" mapTo="RandomStockQuoteService"&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;lifetime type="singleton"/&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;property name="Logger"/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/register&gt;</span></b>
&lt;/container&gt;
```


4. Remove the **register** element mapping the **IStockQuoteService** interface from the **application** container element.
```other
&lt;container name="application"&gt;
  &lt;register type="ILogger" mapTo="TraceSourceLogger"&gt;
    &lt;lifetime type="singleton"/&gt;
    &lt;constructor&gt;
      &lt;param name="traceSourceName"&gt;
        &lt;value value="default"/&gt;
      &lt;/param&gt;
    &lt;/constructor&gt;
  &lt;/register&gt;
  <b><s><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register type="IStockQuoteService" mapTo="RandomStockQuoteService"&gt;</span></s></b>
<b>    <s><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;lifetime type="singleton"/&gt;</span></s></b>
<b>    <s><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;property name="Logger"/&gt;</span></s></b>
<b>  <s><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/register&gt;</span></s></b>
&lt;/container&gt;
```




### Running the Application ###
**To run the application**

1. Launch the application. Open two browser instances, one of them set for private browsing,  and open the application's URL in them. Use the application for a while in both browsers, subscribing to different sets of symbols.
2. Close one of the browser instances and wait for 90 seconds. The session timeout interval is set to one minute in the configuration file, so this wait should be enough for the session to expire.
3. Close the other browser instance, and then stop the development Web server.
4. Open the trace.log file located in the StocksTicker folder. The contents at the bottom of the file should look like the following log, with entries for each of the service instances and for the shared logger.
```other
default Information: 0 : Generating random quotes
    DateTime=2013-08-21T16:29:19.1468695Z
default Information: 0 : Generating random quotes
    DateTime=2013-08-21T16:29:49.1150004Z
default Information: 0 : Generating random quotes
    DateTime=2013-08-21T16:30:19.2251892Z
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">default Information: 0 : Shutting down service</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    DateTime=2013-08-21T16:30:20.0489271Z</span></b>
default Information: 0 : Generating random quotes
    DateTime=2013-08-21T16:30:49.3757469Z
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">default Information: 0 : Shutting down service</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    DateTime=2013-08-21T16:30:50.5663670Z</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">default Information: 0 : Shutting down logger</span></b>
<span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">    DateTime=2013-08-21T16:30:50.5793691Z</span> 
```


To verify you have completed the lab correctly, you can use the solution provided in the Lab05\end\StocksTicker folder.  

