---
Source File Name: C:\code\pandp\git\markdown\sampleDocx\Unity Instructions CS template.docx
AssetID: 7f71b7eb-7ff6-4828-93ca-e3e33f8adea1
Title: Lab 1: Using a Unity Container
Order In ToC: 6-1
Output Filename: 6-1_Lab 1- Using a Unity Container.markdown
---

#### Markdown Test ####
# Lab 1: Using a Unity Container #
----------

Estimated time to complete this lab: **15 minutes**  


## Introduction S##
In this lab, you will practice using a Unity container to create application objects and wire them together. You will update a simple stocks ticker application to replace calls to constructors and property setters with requests to a properly configured Unity container.   
To begin this lab, open the StocksTicker.sln file located in the \Lab01\begin\StocksTicker folder.  


### Running the Application ###
Start by launching the application. To launch the application, on the Visual Studio **Debug** menu click **Start Without Debugging **or press Ctrl-F5. This opens a form and a console window. The console window opens to display the messages logged to the console while the application runs.  
On the application form, you can enter stock quote symbols, consisting of only letters, and subscribe to them by clicking the **Subscribe** button. When the **Refresh** check box is selected, the form displays the latest information about each of the subscribed stock symbols. The form will flash each time an update for a stock symbol is received. The following figure illustrates a sample stocks ticker application.  
<img src="images\577655BE6CC28623542E94D1060D1BD2.jpg" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp" />  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>Using the application's default configuration, the actual stock information is retrieved from the **RandomStockQuoteService, **which randomly generates stock information.</td></tr></table><p /></div>By default, the console is used to log information about the retrieval of updated quotes. The ui.log file (located in the StocksTicker\bin\Debug folder) contains information about the operations performed by the user interface (UI). After you close the application, you can review the contents of the log file.  


### Reviewing the Application ###
The application is built by using the Model-View-Presenter (MVP) pattern. For information about the MVP pattern, see <a href="http://msdn.microsoft.com/en-us/library/ff921132(v=pandp.20).aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Separated Presentation</a> on MSDN. The following figure**Error! Reference source not found.**1shows the classes involved in the application and their relationships.  
<img src="images\337DAD3165543C624D5DEA089A1B8E88.jpg" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp" />  
The **StocksTickerForm** class implements the user interface, and the **IStockQuoteService** interface defines a service for retrieving current stock quotes, the **RandomStockQuoteService **is the concrete implementation of **IStockQuoteService** that is used in these labs. The **ILogger** interface and its implementations are used to log messages about the operation of the application. Finally, the **StocksTickerPresenter** class plays the presenter role.  
All classes in the solution, including the presenter class, have constructors that take all of the required collaborators as parameters and properties where optional collaborators can be set. In the presenter's case, the required collaborators are its view and the service used to poll for updates, and logger is an optional collaborator.  
The static **Program.Main()** method creates all the involved objects in order and connects them together before launching the user interface (UI). The purpose of this lab is to replace the explicit creation of these objects with the use of a Unity container.  


## Task 1: Using a Container S##
In this task, the application's startup code will be updated to use a Unity container to create and connect the application's objects. This will replace the use of explicit calls to the classes' constructors and property setters.  


### Adding References to the Required Assemblies ###
1. In Solution Explorer, select the **StocksTicker** project, and then click **Manage NuGet Packages** on the **Project** menu. 
2. Select the “Online” option to view NuGet Packages available online. 
3. Search for **EntLib6** in the search bar. Select **Unity** and click install.<img src="images\B23E2FF0518319C3D5C7C9C4B0F176D3.png" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp" />  

4.  Click **Accept** on the License Acceptance window that pops up.


### Update the Startup Code to Use a Container ###
**To update the startup code to use a container**

1. Open the **Program.cs **file.
2. Add a **using** directive for the **Unity** namespace.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>using Microsoft.Practices.Unity;</pre></td></tr>
            </table>
          </span>
        </div>
      
3. Replace the creation of the instances (view, presenter, service, loggers) with a **Resolve** request to a new **UnityContainer** for the **StocksTickerPresenter**, as shown in the following highlighted code.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>static void Main()
{
  Application.EnableVisualStyles();
  Application.SetCompatibleTextRenderingDefault(false);

<b>  <span class="highlight">using (IUnityContainer container = new UnityContainer())</span></b>
<b>  <span class="highlight">{</span></b>
    StocksTickerPresenter presenter 
<b>             = <span class="highlight">container.Resolve&lt;StocksTickerPresenter&gt;();</span></b>

    Application.Run((Form)presenter.View);
<b>  <span class="highlight">}</span></b>
}</pre></td></tr>
            </table>
          </span>
        </div>
      Unity containers implement the **IDisposable** interface. This enables the new code to take advantage of the **using** statement to dispose the new container. <a href="#Lab2" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Lab 2</a> elaborates on the result of disposing a container.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>These changes are enough to successfully build the application.  However, even though the Unity container can build objects using the default rules ("autowiring"), additional set up is required before the container can successfully resolve this application's objects.</td></tr></table><p /></div>
4. Debug the application. To do this, click **Start Debugging** on the Visual Studio **Debug** menu or Press F5. The debugger will break with an unhandled **ResolutionFailedException**. This indicates that the attempt to resolve the **StocksTickerPresenter** has failed. Although the container requires additional configuration to resolve the requested presenter, the messages for the **ResolutionFailedException** and its chain of **InnerExceptions **reveal some interesting details:  
+ The request to resolve the presenter is identified with the build key **type = "StocksTicker.UI.StocksTickerPresenter", name = "(none)"**. In this case no name has been provided.
+ The error occurred while resolving an object.
+ The container determined that the constructor for **StocksTickerPresenter** with the signature **(IStocksTickerView view, IStockQuoteService stockQuoteService)** should be used to create the object, by using the autowiring rules.
+ The root cause of the failure was the inability to build an instance of the **IStocksTickerView** interface to be the value for the **view** parameter in the chosen **StocksTickerPresenter** constructor. This problem is indicated by the **InvalidOperationException**
For information about Unity's autowiring rules, see the topic "<a href="http://go.microsoft.com/fwlink/p/?LinkID=317502" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Annotating Objects for Constructor Injection</a>" in the Unity 3 documentation.  

5. Add the required type mappings by using the **RegisterType** method, as shown in the following highlighted code .This will enable the container to resolve the required objects.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>using (IUnityContainer container = new UnityContainer())
{
<b>  <span class="highlight">container</span></b>
<b>      <span class="highlight">.RegisterType&lt;IStocksTickerView, StocksTickerForm&gt;()</span></b>
<b>      <span class="highlight">.RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;();</span></b>

  StocksTickerPresenter presenter
            = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}</pre></td></tr>
            </table>
          </span>
        </div>
      Using the **RegisterType** method is primarily how you will set up a container. In addition to mapping abstract types to concrete ones as described in this step, you can use the **RegisterType** method to override the default injection rules and specify lifetime managers. The next exercises will show additional uses of the **RegisterType** method. For details about mapping types see the topic "<a href="http://go.microsoft.com/fwlink/p/?LinkID=317503" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Registering Types and Type Mappings</a>" in the Unity 3 documentation.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>There are no mappings for the **ILogger** interface. They are not required at this point because properties are not injected unless they are explicitly configured for injection. The application will run now because the involved objects' required collaborators can be resolved by the container, Logging will still not be available because loggers will not be injected into the resolved objects because loggers are optional collaborators in this application.
</td></tr></table><p /></div>


### Running the Application ###
Launch the application and use it. The application behaves as it did before the changes, except that no logging will be available in the console or the ui.log file.  
<a name="_Toc223172411" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Task 2: Using Attributes to Control Injection S##
Attributes can be used to override the default injection rules. In some cases, attributes are used to opt-in for injection on members ignored by the default rules. In other cases, attributes must be used to override the default rules or disambiguate cases where the rules cannot be used.  


### Using Attributes to Enable Property Injection ###
It is very common to have the container set properties, in addition to passing in dependencies by using constructor arguments.  
<a name="_Set_up_injection" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="AnnotateProperty" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>**To set up injection for the Logger property on the RandomStockQuoteService class**

1. Open the StockQuoteServices\RandomStockQuoteService.cs file.
2. Add a **using** directive for the Unity namespace.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>using Microsoft.Practices.Unity;</pre></td></tr>
            </table>
          </span>
        </div>
      
3. Add the **Dependency** attribute to the **Logger** property, as shown in the following highlighted code.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>private ILogger logger;
<b><span class="highlight">[Dependency]</span></b>
public ILogger Logger
{
    get { return logger; }
    set { logger = value; }
}</pre></td></tr>
            </table>
          </span>
        </div>
      <div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>The **Dependency** attribute can be used to indicate that the property should be injected, which results in resolving the property's type, **ILogger**, in the container. For information about property (setter) injection, see the topic "<a href="http://go.microsoft.com/fwlink/p/?LinkID=317504">Annotating Objects for Property (Setter) Injection</a>" in the Unity 3 documentation. 
</td></tr></table><p /></div>


### Adding a Type Registration to Resolve the ILogger Interface ###
Because the container must now resolve the **ILogger** interface, a mapping from the interface to a concrete type must be added to the container. In this example, the interface will be mapped to the **ConsoleLogger** class in order to match the code originally used to wire-up the objects.  
**To add a type registration to resolve the ILogger interface**

1. Open the Program.cs** **file.
2. Use the **RegisterType** method to map the **ILogger **interface to the **ConsoleLogger** class, as shown in the following highlighted code.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>using (IUnityContainer container = new UnityContainer())
{
  container
    .RegisterType&lt;IStocksTickerView, StocksTickerForm&gt;()
    .RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;()
<b>    <span class="highlight">.RegisterType&lt;ILogger, ConsoleLogger&gt;()</span></b>;

  StocksTickerPresenter presenter
            = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}</pre></td></tr>
            </table>
          </span>
        </div>
      
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>Notice how the new call to the **RegisterType** method is chained to the previous calls.</td></tr></table><p /></div>

### Running the Application ###
Launch and use the application. Operations performed by the service are logged to the console, but UI operations are not logged to the console because the **Logger** property on the presenter class is still not injected.  


### Setting Up Injection for the Logger Property on the StocksTickerPresenter Class ###
To inject the **Logger** property on the presenter class, this lab uses the same **Dependency** attribute. However, because a different logger instance is to be injected, you need a way to differentiate the loggers. The Unity container lets you register the same type multiple times, and give each registration a different name. When that dependency is resolved, the name can be used to specify exactly which instance you want. In this lab, you will use different names so that you can tell the difference between the logger objects.   
**To set up injection for the Logger property on the StocksTickerPresenter class**

1. Open the UI\ StocksTickerPresenter.cs file.
2. Add a using directive for the Unity namespace.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>using Microsoft.Practices.Unity;</pre></td></tr>
            </table>
          </span>
        </div>
      
3. Add the **Dependency** attribute to the **Logger** property using "**UI"** for the name, as shown in the following highlighted code.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>private ILogger logger;

<b><span class="highlight">[Dependency("UI")] </span></b>
public ILogger Logger
{
  get { return logger; }
  set { logger = value; }
}</pre></td></tr>
            </table>
          </span>
        </div>
      


### Adding a Type Registration to Resolve the ILogger Interface with the "UI" Name ###
**To add a type registration to resolve the ILogger interface with the "UI" name**

1. Open the Program.cs** **file.
2. Use the **RegisterType** method to map the **ILogger** interface to the **TraceSourceLogger** class with the "**UI"** name, as shown in the following highlighted code.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>using (IUnityContainer container = new UnityContainer())
{
  container
    .RegisterType&lt;IStocksTickerView, StocksTickerForm&gt;()
    .RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;()
    .RegisterType&lt;ILogger, ConsoleLogger&gt;()
<b>    <span class="highlight">.RegisterType&lt;ILogger, TraceSourceLogger&gt;("UI")</span></b>;

  StocksTickerPresenter presenter
            = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}</pre></td></tr>
            </table>
          </span>
        </div>
      <div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>The name parameter for the **RegisterType** method is optional. The default is **null**.
</td></tr></table><p /></div>


### Debugging the Application ###
Launch the application. The debugger should break with an unhandled exception that indicates a new failure while trying to resolve the **Logger** property on the presenter to an instance of **TraceSourceLogger**. Examining the chain of **InnerExceptions** shows that the root cause of the failure is indicated with an **InvalidOperationException** and the message "The type TraceSourceLogger has multiple constructors of length 1. Unable to disambiguate." In this case, Unity's default injection rules cannot determine how to build the instance, so the container must be explicitly configured to build it.   


### Indicating Which Constructor to Use When Building an Instance of TraceSourceLogger with the InjectionConstructor Attribute ###
This lab uses an attribute to indicate which of the two available constructors should be used to create an instance of the **TraceSourceLogger** class.  
**To indicate which constructor to use when building an instance of TraceSourceLogger with the InjectionConstructor attribute**

1. Open the Loggers\TraceSourceLogger.cs file.
2. Add a **using** directive for the Unity namespace.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>using Microsoft.Practices.Unity;</pre></td></tr>
            </table>
          </span>
        </div>
      
3. Add the **InjectionConstructor** attribute to the constructor that takes a **TraceSource** as its sole parameter, as shown in the following highlighted code.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre><b><span class="highlight">[InjectionConstructor]</span></b>
public TraceSourceLogger(TraceSource traceSource)
{
  this.traceSource = traceSource;
}</pre></td></tr>
            </table>
          </span>
        </div>
      


### Adding an Instance Registration to Resolve the TraceSource Type ###
After being pointed to one of the constructors, Unity's default injection rules take effect and an instance of the .NET Framework **TraceSource** class will be resolved to be used as the argument for the constructor call. Instead of instructing the container about how to build such an instance, which would be problematic since the class cannot be annotated with attributes, a pre-built instance will be supplied to the container.  
**To add an instance registration to resolve the TraceSource type**

1. Open the Program.cs file.
2. Use the **RegisterInstance** method to indicate the instance to return when resolving the **TraceSource **class, as shown in the following highlighted code.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>using (IUnityContainer container = new UnityContainer())
{
  container
    .RegisterType&lt;IStocksTickerView, StocksTickerForm&gt;()
    .RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;()
    .RegisterType&lt;ILogger, ConsoleLogger&gt;()
    .RegisterType&lt;ILogger, TraceSourceLogger&gt;("UI")
<b>    <span class="highlight">.RegisterInstance(new TraceSource("UI", SourceLevels.All));</span></b>

  StocksTickerPresenter presenter
           = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}</pre></td></tr>
            </table>
          </span>
        </div>
      
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>The container has generic and non-generic versions of the **RegisterInstance** method. In this task, the generic version is used, relying on the compiler's inference to avoid specifying the generic type argument. This works in this task because the type of the expression used to obtain the reference to register has the same type (**TraceSource**) that will be resolved by the container. If there was a need for a mapping—for example, if an instance is registered to supply the implementation for an interface—the generic type argument should have been provided.</td></tr></table><p /></div>

### Running the Application ###
Launch and use the application. Now the application should work exactly as it did initially, with messages from the UI being logged to the ui.log file (located in the StocksTicker\bin\Debug folder) and messages from the service being logged to the console.  
Attributes are a convenient mechanism to override or disambiguate the container's default injection rules but can result in brittle dependencies. Using names can make particularly brittle dependencies because they get hard-coded in the source code. The next labs show alternative mechanisms to externalize the setup of a container without involving the objects being created. To verify you have completed the lab correctly, you can use the solution provided in the \Lab01\end\StocksTicker folder.
  
