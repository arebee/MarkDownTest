---
Source File Name: C:\code\pandp\git\markdown\sampleDocx\Unity Instructions CS template.docx
AssetID: 1aae6b9d-d436-4685-b2d9-576c59017ee8
Title: Lab 2: Configuring Injection Using the Configuration API
Order In ToC: 6-2
Output Filename: 6-2_Lab 2- Configuring Injection Using the Configuration API.markdown
---
#### Markdown Test ####
# Lab 2: Configuring Injection Using the Configuration API #
----------

Estimated time to complete this lab: **20 minutes**  


## Introduction S##
In this lab, you will practice configuring a container to perform dependency injection at run time, without relying on annotating the classes to resolve with attributes and set up lifetime managers.  
To begin, open the StocksTicker.sln file located in the Lab02\begin\StocksTicker folder. This is the same application used in Lab 1.  
<a name="_Toc223172414" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Task 1: Configuring the Container Using the Fluent API S##


### Updating the Container Configuration to Override the Default Injection Rules ###
Throughout this task, calls to the **RegisterType** method will be added or modified in order to configure the container.   
To provide injection configuration, calls to the container's **RegisterType** methods receive **InjectionMember** objects. **InjectionMember** is a base class. Its subclasses **InjectionProperty **and **InjectionConstructor** are used in this lab. For information about configuring injection in a Unity container, see the topic "<a href="http://go.microsoft.com/fwlink/p/?LinkID=317505" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Registering Injected Parameter and Property Values</a>" in the Unity 3 documentation.  
**To update container configuration to override the default injection rules**

1. Open the **Program.cs **file.
2. Update the **RegisterType** call for the **IStockQuoteService** interface in order to inject the **Logger** property using the **InjectionProperty** object, as shown in the following highlighted code.
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
    .RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;(
<b>                  <span class="highlight">new InjectionProperty("Logger"))</span></b>
    .RegisterType&lt;ILogger, ConsoleLogger&gt;()
    .RegisterType&lt;ILogger, TraceSourceLogger&gt;("UI")
    .RegisterInstance(new TraceSource("UI", SourceLevels.All));

  StocksTickerPresenter presenter
            = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}</pre></td></tr>
            </table>
          </span>
        </div>
      <div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>The **InjectionProperty** object as configured in the preceding code indicates that the property with the name "**Logger"** should be injected. Because there is no further configuration for this property, the unnamed instance for the property's type, **ILogger**, will be resolved when obtaining the value to inject to the property. This is similar to the previous lab which used the **Dependency** attribute without further configuration to <a href="test-markdown_7f71b7eb-7ff6-4828-93ca-e3e33f8adea1.html#AnnotateProperty">annotate the property</a>.</td></tr></table><p /></div>
3. Use the **RegisterType** method to set up the injection of the **StocksTickerPresenter** class, as shown in the following highlighted code.
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
    .RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;(
                  new InjectionProperty("Logger"))
    .RegisterType&lt;ILogger, ConsoleLogger&gt;()
    .RegisterType&lt;ILogger, TraceSourceLogger&gt;("UI")
    .RegisterInstance(new TraceSource("UI", SourceLevels.All))
<b>    <span class="highlight">.RegisterType&lt;StocksTickerPresenter&gt;(</span></b>
<b>            <span class="highlight">new InjectionProperty("Logger", </span></b>
<b>                <span class="highlight">new ResolvedParameter&lt;ILogger&gt;("UI")));</span></b>

  StocksTickerPresenter presenter
            = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}</pre></td></tr>
            </table>
          </span>
        </div>
      <div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>In this case, the **RegisterType** call is not used to perform a mapping. It is used only to inject the **Logger** property. Because of this, there is a single generic type argument. Additionally, the **InjectionProperty** is configured with a **ResolvedParameter**. This parameter is used to indicate the name of the instance to resolve (which was not necessary in the previous step, so it was omitted.)</td></tr></table><p /></div>This configuration is sufficient to successfully run the application. The **InjectionConstructor** attribute drives the creation of the **TraceSourceLogger** object, injecting it with the registered **TraceSource** instance. However, there are many cases where you cannot apply an attribute or where you want to override the attribute and inject something different. The next step shows how to use the **RegisterType** method to override the default creation rules and use a different constructor to create the **TraceSourceLogger**.  

4. Update the **RegisterType** call for the **ILogger** interface with the "**UI"** name in order to inject the constructor with a single string parameter, as shown in the following highlighted code.
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
    .RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;(
                  new InjectionProperty("Logger"))
    .RegisterType&lt;ILogger, ConsoleLogger&gt;()
<b>    <span class="highlight">.RegisterType&lt;ILogger, TraceSourceLogger&gt;(</span></b>
<b>                           <span class="highlight">"UI",</span></b>
<b>                           <span class="highlight">new InjectionConstructor("UI"))</span></b>
    .RegisterInstance(new TraceSource("UI", SourceLevels.All))
    .RegisterType&lt;StocksTickerPresenter&gt;(
                  new InjectionProperty("Logger",
                      new ResolvedParameter&lt;ILogger&gt;("UI")));

  StocksTickerPresenter presenter
            = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}</pre></td></tr>
            </table>
          </span>
        </div>
      <div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />TheInjectionConstructor indicates which constructor to use based onits arguments. In this case, the supplied"UI"string causes the constructor ofTraceSourceLogger with a single string parametertobe called, passing the"UI" value as its argument and taking precedence over theconstructorannotatedwith theInjectionConstructor attribute. There are three kinds of argumentsforanInjectionConstructor (and all other members of theInjectionMember hierarchy):</th></tr><tr><td>instances of concrete subclasses of the **InjectionParameterValue** class, instances of the **Type **class, and the remaining objects. For information about how these values are interpreted, see the topic "<a href="http://go.microsoft.com/fwlink/p/?LinkID=317505">Registering Injected Parameter and Property Values</a>" in the Unity 3 documentation.</td></tr></table><p /></div>
5. Remove the **RegisterInstance** call, which is no longer necessary.
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
    .RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;(
                  new InjectionProperty("Logger"))
    .RegisterType&lt;ILogger, ConsoleLogger&gt;()
    .RegisterType&lt;ILogger, TraceSourceLogger&gt;(
                           "UI",
                           new InjectionConstructor("UI"))
<b><s>    <span class="highlight">.RegisterInstance(new TraceSource("UI", SourceLevels.All))</span></s></b>
    .RegisterType&lt;StocksTickerPresenter&gt;(
                  new InjectionProperty("Logger",
                      new ResolvedParameter&lt;ILogger&gt;("UI")));

  StocksTickerPresenter presenter
            = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}</pre></td></tr>
            </table>
          </span>
        </div>
      <div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>The container needs information on how to resolve an instance of the **TraceSource** class in order to successfully resolve the configured **TraceSourceLogger** using the annotated constructor. Because this constructor is no longer used, the registered instance is no longer necessary. Also note that, using the mechanisms described in this exercise, the container could be provided the necessary information to build a **TraceSource** instance instead of being limited to registering an instance created elsewhere.
</td></tr></table><p /></div>


### Running the Application ###
Launch and use the application. Its behavior is the same as it was before the changes, but now the information required by the container to build the application types does not reside in the types themselves (with the exception of the **InjectionConstructor** attribute in the **TraceSourceLogger** type, which was left in place to illustrate how the API calls took precedence over the attributes).  
<a name="_Exercise_2_–" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc223172415" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Using the **RegisterType** method to configure injection provides a degree of flexibility that is not available when using attributes. However, it also requires injection to be specified for each container.  


## Task 2: Using Lifetime Managers S##
Lifetime managers are used by a container for two purposes:  
+ To make sure the result of resolving a particular identifier is always the same instance for a particular context (for example, the same container, the same thread, or the same HTTP session)
+ To properly dispose the resolved objects
In this task, the built-in **ContainerControlledLifetimeManager** will be used to dispose the **TraceSourceLogger**.  


### Updating the TraceSourceLogger Class to Implement the IDisposable Interface ###
The **TraceSourceLogger**, as implemented at this stage, flushes its **TraceSource** after each message is logged and does not close it. In this task, the class is changed to implement the **IDisposable** interface in order to properly close its trace source.  
**To update the TraceSourceLogger class to implement the IDisposable interface**

1. Open the Loggers\TraceSourceLogger.cs file.
2. Add the **IDisposable** interface to the list of interfaces implemented by the **TraceSourceLogger** class, as shown in following highlighted code.
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
              <tr><td colspan="2"><pre>public class TraceSourceLogger : ILogger<b><span class="highlight">, IDisposable</span></b>
{
  ...
}</pre></td></tr>
            </table>
          </span>
        </div>
      
3. Remove the call to the **Flush** method in the implementation of the **Log** method, as shown in the following highlighted code.
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
              <tr><td colspan="2"><pre>public void Log(string message, TraceEventType eventType)
{
  this.traceSource.TraceEvent(eventType, 0, message);
<b>  <s><span class="highlight">this.traceSource.Flush();</span></s></b>
}</pre></td></tr>
            </table>
          </span>
        </div>
      This change enables buffering of the logged messages by not forcing the trace source to flush each time a message is logged.  

4. Add the following code to implement the **Dispose** method defined in the **IDisposable** interface.
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
              <tr><td colspan="2"><pre>public void Dispose()
{
  if (this.traceSource != null)
  {
    this.traceSource.TraceInformation("Shutting down logger");
    this.traceSource.Close();
    this.traceSource = null;
  }
}</pre></td></tr>
            </table>
          </span>
        </div>
      
This implementation logs a "shutting down" message and closes the trace source.  
<a name="_Update_the_container" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Updating the Container Configuration Calls to Use a Lifetime Manager for the TraceSourceLogger Class ###
**To update the container configuration calls to use a lifetime manager for the TraceSourceLogger class**

+ Update the **RegisterType** call for the **ILogger** interface with the "**UI"** name to use a new **ContainerControlledLifetimeManager **instance, as shown in the following highlighted code.
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
    .RegisterType&lt;IStockQuoteService, RandomStockQuoteService&gt;(
                  new InjectionProperty("Logger"))
    .RegisterType&lt;ILogger, ConsoleLogger&gt;()
    .RegisterType&lt;ILogger, TraceSourceLogger&gt;(
                  "UI",
<b>                  <span class="highlight">new ContainerControlledLifetimeManager(),</span></b>
                  new InjectionConstructor("UI"))
    .RegisterType&lt;StocksTickerPresenter&gt;(
                 new InjectionProperty("Logger",
                     new ResolvedParameter&lt;ILogger&gt;("UI")));

  StocksTickerPresenter presenter
            = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}</pre></td></tr>
            </table>
          </span>
        </div>
      <div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>A lifetime manager is an optional parameter of the **RegisterType** type. For information about lifetime managers in general, and the **ContainerControlledLifetimeManager** in particular, see the topic "<a href="http://go.microsoft.com/fwlink/p/?LinkID=317506">Understanding Lifetime Managers</a>" in the Unity 3 documentation.
</td></tr></table><p /></div>


### Running the Application ###
Launch the application, use it, and then close its main form to ensure the shutdown code is executed. Open the ui.log file (located in the StocksTicker\bin\Debug folder) and ensure the last two lines show an entry like the following.  

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
              <tr><td colspan="2"><pre>UI Information: 0 : Shutting down logger
    DateTime=2009-02-11T19:02:07.8990000Z</pre></td></tr>
            </table>
          </span>
        </div>
      To verify you have completed the lab correctly, you can use the solution provided in the Lab02\end\StocksTicker folder.  

