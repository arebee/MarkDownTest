---
Source File Name: Unity Instructions CS template.docx
AssetID: 731a39ff-2bf3-41d1-a701-c52b83bdef78
Title: Lab 3: Using a Configuration File to Set Up a Container
Order In ToC: 4-3
Output Filename: 4-3_Lab 3- Using a Configuration File to Set Up a Container.markdown
---

#### Markdown Test ####
# Lab 3: Using a Configuration File to Set Up a Container #
----------

Estimated time to complete this lab: **25 minutes**  


## Introduction ##
In this lab, you will practice using settings retrieved from an application's configuration file to set up a Unity container. Setting up a container with settings from a configuration file is similar to setting up a container by invoking the configuration API used in the previous lab. In fact, configuration settings can be thought of as <i>script</i> representing calls to the API.  
To begin, open the StocksTicker.sln file located in the Lab03\begin\StocksTicker folder.  
<a name="_Toc223172418" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Task 1: Using a Configuration File to Store the Configuration for a Container ##
In this exercise, the container set-up code will be replaced with its equivalent representation as configuration file settings.  


### Adding References to the Required Assemblies ###
1. In the Solution Explorer, select the StocksTicker project, and then click **Add Reference… **on the **Project** menu.
2. Select the “Framework” option to view the available .NET Framework assemblies.
3. Find the entry for **System.Configuration** and click the checkbox to add a reference to it.![](images/C84F6EB8AFD55E00592605659CFBF100.png)  

4. Click **OK**.


### Updating the Startup Code to Set Up the Container ###
**To update the startup code to use information from the configuration file to set up the container**

1. Open the Program.cs file.
2. Add a **using** directive for the **Unity.Configuration** namespace.
```other
using Microsoft.Practices.Unity.Configuration;
```


3. Retrieve the Unity configuration section from the configuration file and use it to configure the container. Replace the calls to **RegisterType **with a call to the **LoadConfiguration** method, as shown in the following highlighted code.
```other
using (IUnityContainer container = new UnityContainer())
{
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">container.LoadConfiguration();</span></b>

  StocksTickerPresenter presenter
           = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}
```

The **LoadConfiguration** extension method is the simplest way to configure a container. It retrieves the configuration information for the default container element from the unity configuration section in the executing application’s configuration file. There are alternative mechanisms for more sophisticated scenarios such as retrieving the configuration from configuration files other than the executing application’s configuration file.  

![](images/note.gif)Note:&gt; For information about how to load configuration information into a container, see "Loading Configuration File Information into a Container" in "[Using Design-Time Configuration](http://go.microsoft.com/fwlink/p/?LinkID=317507)" in the Unity 3 documentation.

### Updating the Configuration File with the Container Configuration ###
Only a subset of Unity's configuration elements is used during this task. For an overall description of Unity's configuration schema, see the topic "[The Unity Configuration Schema](http://go.microsoft.com/fwlink/p/?LinkID=317489)" in the Unity 3 documentation.   
**To update the configuration file with the container configuration**

1. Open the application’s configuration, App.config, file.
2. Add a declaration for the **unity **configuration section.
```other
&lt;configSections&gt;
  ...
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;section name="unity"</span></b>
<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">           type="</span> <b>Microsoft.Practices.Unity.Configuration.UnityConfigurationSection, Microsoft.Practices.Unity.Configuration"/&gt;</b></b>
&lt;/configSections&gt;
```

![](images/note.gif)Note:&gt; The name "**unity**" for the configuration section is only a convention; any name is valid, as long as it matches the name used to retrieve the configuration at run time. Most simplified configuration access methods assume the "unity" section name.
3. Add the **unity** section element.
```other
  &lt;/system.diagnostics&gt;
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;unity xmlns="http://schemas.microsoft.com/practices/2010/unity"&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/unity&gt;</span></b>
&lt;/configuration&gt;
```

The section's element name must match the name used to register the section in the **configSections** collection.  
![](images/note.gif)Note:&gt; The **xmlns** attribute is not required by the configuration runtime. If you do set it and use **"unity"** as the section name it enables the Visual Studio XML editor, which is used by default to edit configuration files, to take advantage of the Unity Configuration XSD file installed with Unity to provide support for IntelliSense. This greatly simplifies the authoring of configuration information involving the Unity container.
4. Add an** alias** element for the **TraceSource** type.
```other
&lt;unity xmlns="http://schemas.microsoft.com/practices/2010/unity"&gt;
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;alias</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">alias="TraceSource"</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">type="System.Diagnostics.TraceSource, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" /&gt;</span></b>
&lt;/unity&gt;
```

![](images/note.gif)Note:&gt; Type aliases are not required. Type aliases and the assembly and namespace elements typically make the configuration files less verbose and easier to read.  Aliases for common types, such as .NET framework types like **int** and **string** or Unity types like **singleton**, are built-in so you do not need to define them. For information about the built-in aliases, see "Default Aliases and Assemblies" in "[Specifying Types in the Configuration File](http://go.microsoft.com/fwlink/p/?LinkID=317508)" in the Unity 3 documentation.
5. Add an **assembly** element for the application’s assembly and **namespace** elements for the namespaces containing types that are used to configure the container.
```other
&lt;unity xmlns="http://schemas.microsoft.com/practices/2010/unity"&gt;
  &lt;alias
    alias="TraceSource"
    type="System.Diagnostics.TraceSource, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" /&gt;

<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;assembly name="StocksTicker"/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;namespace name="StocksTicker.Loggers"/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;namespace name="StocksTicker.StockQuoteServices"/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;namespace name="StocksTicker.UI"/&gt;</span></b>
&lt;/unity&gt;
```

![](images/note.gif)Note:&gt; While the **alias **element can be used to assign an arbitrary name to a single type, the **assembly **and **namespace **elements indicate the configuration run-time in which assemblies and namespaces search for types for which only the name is specified in the configuration file.
6. Add an element for the default (unnamed) **container**.
```other
  &lt;namespace name="StocksTicker.UI"/&gt;

  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;container&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/container&gt;</span></b>
&lt;/unity&gt;
```

![](images/note.gif)Note:&gt; Use of the default container only differs from named containers, in that it is used if no container name is supplied when loading the configuration.
7. Add a **register** element to map the **IStocksTickerView **interface to the **StocksTickerForm** class.
```other
&lt;container&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register type="IStocksTickerView" mapTo="StocksTickerForm"/&gt;</span></b>
&lt;/container&gt;
```

![](images/note.gif)Note:&gt; Notice only the type names are used with no indication of namespace or assembly. The full type names could have also been used but the configuration run-time will be able to retrieve the right types by using the information in the **namespace** and **assembly** elements.
8. Add a **register** element to map the **IStockQuoteService** interface** **to the **RandomStockQuoteService** class, with a child **property** element to configure the **Logger** property to be injected.
```other
&lt;container&gt;
  &lt;register type="IStocksTickerView" mapTo="StocksTickerForm"/&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register type="IStockQuoteService" mapTo="RandomStockQuoteService"&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;property name="Logger"/&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/register&gt;</span></b>
&lt;/container&gt;
```


9. Add a **register** element to map the **ILogger** interface to the **ConsoleLogger** class.
```other
&lt;container&gt;
  &lt;register type="IStocksTickerView" mapTo="StocksTickerForm"/&gt;
  &lt;register type="IStockQuoteService" mapTo="RandomStockQuoteService"&gt;
    &lt;property name="Logger"/&gt;
  &lt;/register&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register type="ILogger" mapTo="ConsoleLogger"/&gt;</span></b>
&lt;/container&gt;
```


10. Add a **register** element to map the **ILogger** interface to the **TraceSourceLogger** using the "**UI**" name. Add a child **constructor** element to indicate the use of the constructor with a single parameter named **traceSourceName** to build the instance, and pass the "UI" string as the argument with the **value** attribute. Use a child **lifetime** element to indicate the use of the **ContainerControlledLifetimeManager** through the use of the built-in **singleton** alias.
```other
&lt;container&gt;
  &lt;register type="IStocksTickerView" mapTo="StocksTickerForm"/&gt;
  &lt;register type="IStockQuoteService" mapTo="RandomStockQuoteService"&gt;
    &lt;property name="Logger"/&gt;
  &lt;/register&gt;
  &lt;register type="ILogger" mapTo="ConsoleLogger"/&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register name="UI" type="ILogger" mapTo="TraceSourceLogger"&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;lifetime type="singleton"/&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;constructor&gt;</span></b>
<b>      <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;param name="traceSourceName" value="UI"/&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/constructor&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/register&gt;</span></b>
&lt;/container&gt;
```

![](images/note.gif)Note:&gt; The **constructor** element is equivalent to the **InjectionConstructor** class used in the [previous lab](test-markdown_1aae6b9d-d436-4685-b2d9-576c59017ee8.html#_Update_the_container). The **value** attribute, used as a shortcut for the **value** child element, indicates that its value should be supplied as the value for the parameter it represents. If necessary, it is converted to the parameter's type. For information about the **value** element, see "[The value Element](http://go.microsoft.com/fwlink/p/?LinkID=317509)" in "[The Unity Configuration Schema](http://go.microsoft.com/fwlink/p/?LinkID=317489)" the Unity 3 documentation. 
11. Add a **register** element for the **StocksTickerPresenter** class with a child **property **element. The **property** element configures the **Logger** property to be injected with the value of resolving the **ILogger** interface (the property’s type), with the "**UI**"** **name (which requires using the **dependency** element, as shown below. Alternatively you could use the **dependencyName** attribute on the **property** element.
```other
&lt;container&gt;
  &lt;register type="IStocksTickerView" mapTo="StocksTickerForm"/&gt;
  &lt;register type="IStockQuoteService" mapTo="MoneyCentralStockQuoteService"&gt;
    &lt;property name="Logger"/&gt;
  &lt;/register&gt;
  &lt;register type="ILogger" mapTo="ConsoleLogger"/&gt;
  &lt;register name="UI" type="ILogger" mapTo="TraceSourceLogger"&gt;
    &lt;lifetime type="singleton"/&gt;
    &lt;constructor&gt;
      &lt;param name="traceSourceName" value="UI"/&gt;
    &lt;/constructor&gt;
  &lt;/register&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register type="StocksTickerPresenter"&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;property name="Logger"&gt;</span></b>
<b>      <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;dependency name="UI"/&gt;</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/property&gt;</span></b>
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;/register&gt;</span></b>
&lt;/container&gt;
```

![](images/note.gif)Note:&gt; Notice how "**UI**" is used to indicate the name of the instance to resolve, while it is used as a literal string value in the previous step. Also note the absence of the **mapTo** attribute in the **register** element. The **dependency** element indicates that the value of the **name** attribute ("UI" in this case) is to be used, together with the property's type, as the key to resolve the object to inject into the property. This element is an alternative to the **value** attribute used in the previous step; for details on the **dependency** element see the topic "[The dependency Element](http://go.microsoft.com/fwlink/p/?LinkID=317510)" in "[The Unity Configuration Schema](http://go.microsoft.com/fwlink/p/?LinkID=317489)" in the Unity 3  documentation. 



### Running the Application ###
Launch and use the application. It behaves as it did before the changes, including the logging behavior.  
Now all the information required to set up the container is stored in the configuration file; no code changes are required to change the way the container is to resolve the application objects.  
To verify you have completed the lab correctly, you can use the solution provided in the Lab03\end\StocksTicker folder.  

