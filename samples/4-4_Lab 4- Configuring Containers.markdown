---
Source File Name: Unity Instructions CS template.docx
AssetID: dd4199f2-b8f2-4db7-b038-a5ff9b0344c3
Title: Lab 4: Configuring Containers
Order In ToC: 4-4
Output Filename: 4-4_Lab 4- Configuring Containers.markdown
---

#### Markdown Test ####
# Lab 4: Configuring Containers #
----------

Estimated time to complete this lab: **15 minutes**  


## Introduction ##
In this lab, you will practice using some of Unity's more advanced features: generic decorator chains, resolver overrides and array injection.  
The application used in this lab is an updated version of the stocks ticker application used in the previous labs, with the additional requirement to save updated stock quotes to a persistent repository using a third-party <i>persistence framework</i>. This persistence framework defines a generic **IRepository&lt;&gt;** interface, and a concrete generic **DebugRepository&lt;&gt;** class will be used in this lab.  
To begin, open the StocksTicker.sln file located in the Lab04\begin\StocksTicker folder.  


## Task 1: Configuring Open Generics to Resolve Closed Generics ##
A Unity container can be configured using closed generic types, which work exactly like non-generic types, or using open generic types. In the latter case, configuration applies to any closed generic type built from the open one as long as there is not a more specific configuration for the closed generic type.  


### Reviewing Code ###
**To review the code**

1. Open the UI\StocksTickerPresenter.cs file.
2. Note the changes in the code.
The **StocksTickerPresenter **is injected with a suitable repository through a new constructor parameter.  

```other
public StocksTickerPresenter(
  IStocksTickerView view,
  IStockQuoteService stockQuoteService,
<b>  <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">IRepository&lt;StockQuote&gt; repository</span></b>)
{
    ...
}
```

The repository is used to store updated stock quotes.  

```other
private void SaveQuote(StockQuote updatedQuote)
{
  try
  {
    this.repository.Save(updatedQuote);
  }
  catch (RepositoryException e)
  {
     this.logger.Log(
          string.Format(
                "Error saving the updated quote for '{0}': {1}",
                updatedQuote.Symbol,
                e.Message),
          TraceEventType.Warning);
  }
}
```



### Reviewing the Configuration File ###
**To review the configuration file**

1. Open the App.config file.
2. Note the changes in the configuration file. 
Just as in the previous lab, there is no explicit constructor injection configuration for the **StocksTickerPresenter**: the default injection rules kick in to find the presenter's single constructor and invoke it with the result of resolving each of its parameters. To support a new parameter of type **IRepository&lt;StockQuote&gt;**, a new **register** element maps the closed generic interface to an implementation, which happens to be a closed generic class.  

```other
&lt;register type="IRepository[StockQuote]" mapTo="DebugRepository[StockQuote]"/&gt;
```

For information about how to specify type names involving generics, see the topic "[Specifying Types in the Configuration File](http://go.microsoft.com/fwlink/p/?LinkID=317511)" in the Unity 3 documentation.  


### Running the Application ###
Launch and use the application; each time an update is received for a stock quote, the corresponding entry is logged to the console. Note that loggers are not used in this case; the **DebugRepository&lt;&gt;** class writes output directly through the **Console** class.  


### Updating the Configuration File to Register Open Generic Types ###
**To update the configuration file to register open generic types**

1. Open the App.config file.
2. Replace the **register** element mapping the closed **IRepository** interface to the closed **DebugRepository** class with a new element mapping the open types.
```other
&lt;container&gt;
  &lt;register type="IStocksTickerView" mapTo="StocksTickerForm"/&gt;
  &lt;register type="IStockQuoteService" mapTo="RandomStockQuoteService"&gt;
    ...
  &lt;/register&gt;
  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">&lt;register type="IRepository[]" mapTo="DebugRepository[]"/&gt;</span></b>
  &lt;register type="ILogger" mapTo="ConsoleLogger"/&gt;
  &lt;register name="UI" type="ILogger" mapTo="TraceSourceLogger"&gt;
    ...
  &lt;/register&gt;
  &lt;register type="StocksTickerPresenter"&gt;
    ...
  &lt;/register&gt;
&lt;/container&gt;
```




### Running the Application ###
Launch and use the application; it will run as it did before the changes.  
When resolving a (closed) generic type, the Unity container looks for definitions (such as mappings and injection specifications) for the closed generic type. If no such definitions are found, the container looks for definitions for the open generic type from which the resolved type is created and uses them if they are available, resulting in mapping the generic type arguments from the original closed generic type as appropriate.  
In this exercise, the closed **IRepository&lt;StockQuote&gt;** generic interface needs to be resolved when gathering the arguments for the constructor of the **StocksTickerPresenter** class (following the container's default injection rules). After the configuration change described in this task, no entry is found for the closed generic interface, so an entry for the open **IRepository&lt;&gt;** interface is looked for and found. The container uses this mapping and determines that the open generic **DebugRepository&lt;&gt;** class should be used. The closed type's generic type argument **StockQuote** is applied to the mapped open generic class. This results in resolving the closed **DebugRepository&lt;StockQuote&gt;** class, which is instantiated and used as the constructor argument for the **StocksTickerPresenter**.   
Configuring open generic types helps avoid duplication. Because definitions for closed generic types take precedence over definitions for open generic types, these definitions can be overridden for a specific closed generic type, if necessary.  
<a name="_Toc223172422" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Task 2: Resolving Generic Decorator Chains ##
At the end of Task 1, the runtime structure of the **StocksTickerPresenter** looks like the following schematic.  
![](images/477aecd8-8d77-4e06-8b4f-e1e67f362606.png)  
In this task, the container will be configured to insert a decorator class between the presenter and the repository. The final runtime objects will look like the following schematic.  
![](images/2c5361ab-d744-41d8-9bed-e6ca51dc75f9.png)  
In order to accomplish this, the named registration feature of the container will be used. The names used in each registration are shown in the callout tags in the schematic.  
In this task, the settings from the configuration file will be partially overridden in the application code registering new types using the **RegisterType** method and using **Resolve** overrides. As a result, the **StocksTickerPresenter** will be injected with a **ValidatingRepository&lt;StockQuote&gt;** wrapping the original debug repository instead of getting an instance of **DebugRepository&lt;StockQuote&gt;**. Mixing configuration from different sources is permitted because API calls and settings from the configuration file are equivalent. This **ValidatingRepository&lt;&gt;** class works as a decorator; the container is responsible for figuring out how to properly wire it up.  


### Reviewing the ValidatingRepository Class and the IValidator Implementation for the StockQuote Class ###
**To review the ValidatingRepository class and the IValidator implementation for the StockQuote class**

1. Open the ValidatingRepository.cs file in the PersistenceFramework project.
2. Examine the **ValidatingRepository** class.
```other
public ValidatingRepository(
    IRepository&lt;T&gt; repository, 
    IValidator&lt;T&gt; validator)
{
  ...
}
```

The constructor for the **ValidatingRepository&lt;T&gt;** class receives two parameters, a wrapped repository and a validator. For the purposes of this lab, a validator that relies on random values to determine whether an instance is valid will be used for stock quotes.   

3. Open the RandomStockQuoteValidator.cs file in the StocksTicker project.
4. Examine the **RandomStockQuoteValidator** class. It is a non-generic class that implements the closed **IValidator&lt;StockQuote&gt;** generic interface.


### Updating the Container Setup with API Calls ###
**To update the container setup code with API calls**

1. Open the **Program.cs** file in the **StocksTicker** project.
2. After configuration is applied to the container, use the **RegisterType** method to map the open **IRepository&lt;&gt; **generic** **interface to the open** ValidatingRepository&lt;&gt;** class with the "**validating"** name, as shown in the following code.
```other
using (IUnityContainer container = new UnityContainer())
{
  container.LoadConfiguration();

  <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">container</span></b>
<b>    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">.RegisterType(</span></b>
<b>            <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">typeof(IRepository&lt;&gt;),</span></b>
<b>            <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">typeof(ValidatingRepository&lt;&gt;),</span></b>
<b>            <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">"validating");</span></b>

  StocksTickerPresenter presenter
          = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}
```

![](images/note.gif)Note:&gt; Because open generic types cannot be used as generic type arguments, the version of the **RegisterType** method taking **Type** instances as parameters is used instead of the version with generic type parameters used previously. These versions are mostly equivalent, although the version with generic type parameters benefits from compile-time type checks.
3. Use the **RegisterType** method to map the closed **IValidator&lt;StockQuote&gt; **interface to the non-generic **RandomStockQuoteValidator** class, as shown in the following code.
```other
using (IUnityContainer container = new UnityContainer())
{
  container.LoadConfiguration();

  container
        .RegisterType(
            typeof(IRepository&lt;&gt;),
            typeof(ValidatingRepository&lt;&gt;),
            "validating")
        <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">.RegisterType&lt;IValidator&lt;StockQuote&gt;, RandomStockQuoteValidator&gt;();</span></b>

  StocksTickerPresenter presenter
        = container.Resolve&lt;StocksTickerPresenter&gt;();

  Application.Run((Form)presenter.View);
}
```

![](images/note.gif)Note:&gt; This registration is necessary to resolve the **IValidator&lt;StockQuote&gt;** argument when resolving the **ValidatingRepository&lt;StockQuote&gt;** mapped with the "**validating**" name. Mixing registrations for open and closed generic types is possible; it is also possible to map closed generic types to non-generic types. However, any Resolve request for an **IRepository&lt;T&gt;** with name **validating** will require a proper registration of **IValidator&lt;T&gt;** to avoid a resolve failure.



### Resolve objects using overrides ###
**To resolve objects with overrides**

1. Update the call to the **Resolve** method to supply a new **ParameterOverride** object for the **“repository”** parameter so that the repository with name **“validating”** is resolved and injected.
```other
using (IUnityContainer container = new UnityContainer())
{
  container.LoadConfiguration();

  container
        .RegisterType(
            typeof(IRepository&lt;&gt;),
            typeof(ValidatingRepository&lt;&gt;),
            "validating")
        .RegisterType&lt;IValidator&lt;StockQuote&gt;, RandomStockQuoteValidator&gt;();

  StocksTickerPresenter presenter
        = container.Resolve&lt;StocksTickerPresenter&gt;(
            <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new ParameterOverride(</span></b>
<b>                <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">"repository",</span></b>
<b>                <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new ResolvedParameter&lt;IRepository&lt;StockQuote&gt;&gt;("validating"))</span></b>
<b>                    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">.OnType&lt;StocksTickerPresenter&gt;()</span></b>);

  Application.Run((Form)presenter.View);
}
```

![](images/note.gif)Note:&gt; The **ParameterOverride** object instructs the constructor to use the supplied value when injecting parameters with the supplied name. In this case the override indicates that the repository named **“validating”** should be resolved an injected for parameter **“repository”**. Values for all overrides follow the same rules as values for injection members like **InjectionConstructor**. Overrides apply to any object being resolved, not just the “top level” one. Invoking the **OnType** method on the parameter override constrains the override to a specific type (which may or may not be the type supplied in the **Resolved** call, which is **StocksTickerPresenter** in the example above).



### Running the Application ###
Launch and use the application for some time. Look at the ui.log file, where you should find entries describing a **RepositoryException** thrown by the validating repository, as shown in the following code.  

```other
UI Information: 0 : Refresh timer elapsed
    DateTime=2009-02-16T18:02:28.1595997Z
UI Information: 0 : StockQuote for MSFT was updated
    DateTime=2009-02-16T18:02:29.4395997Z
UI Warning: 0 : <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Error saving the updated quote for 'MSFT': Invalid instance to save</span></b>
    DateTime=2009-02-16T18:02:29.4805997Z
```

<a name="_Toc223172423" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Task 3: Using Array Injection ##
In this task, a **CompositeLogger** will be injected to the resolved **StocksTickerPresenter** instance instead of the **TraceSourceLogger** used earlier. This logger requires an array of loggers and forwards the logging requests it receives to the elements in this array.  
Although an array can be injected as a value, supplying it when configuring injection like any other CLR object, this approach is not always appropriate. Unity can be configured to inject an array, using the result of resolving other keys as elements.  
There are two kinds of array injection:  
+ Injecting an array that contains all the instances of the array's element type registered in the container (in the order they were registered). This is the result of resolving the array type (such as **ILogger[] **in this case).
+ Injecting an array containing the result of resolving specific keys.
The second approach will be used in this task.  


### Updating the Container Setup to Inject a CompositeLogger to the StocksTickerPresenter ###
**To update the container setup to inject a CompositeLogger to the StocksTickerPresenter**

1. Open the Program.cs file.
2. Use the **RegisterType** method to map the **ILogger** interface to the **CompositeLogger** class with the "**composite**" name, as shown in the following code.
```other
using (IUnityContainer container = new UnityContainer())
{
  container.LoadConfiguration();

  container
        .RegisterType(
            typeof(IRepository&lt;&gt;),
            typeof(ValidatingRepository&lt;&gt;),
            "validating")
        .RegisterType&lt;IValidator&lt;StockQuote&gt;, RandomStockQuoteValidator&gt;()
        <b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">.RegisterType&lt;ILogger, CompositeLogger&gt;("composite");</span></b>

  StocksTickerPresenter presenter
        = container.Resolve&lt;StocksTickerPresenter&gt;(
            new ParameterOverride(
                "repository",
                new ResolvedParameter&lt;IRepository&lt;StockQuote&gt;&gt;("validating"))
                    .OnType&lt;StocksTickerPresenter&gt;());

  Application.Run((Form)presenter.View);
}
```

![](images/note.gif)Note:&gt; Without additional configuration, the default injection rules indicate that the container would attempt to resolve the argument for the **CompositeLogger**'s constructor of type **ILogger[]**. This would result in an attempt to resolve all the instances registered to the container with a name, which include the **CompositeLogger** being built in the first place, thus resulting in a **StackOverflowException** caused by unbounded recursion; this issue would not occur if the **CompositeLogger** had not been registered with a name, because only named instances are included when resolving an array.
3. Update the **RegisterType** call, mapping the **ILogger** interface to the **CompositeLogger** class to inject an array of specific instances through the constructor.
```other
using (IUnityContainer container = new UnityContainer())
{
  container.LoadConfiguration();

  container
        .RegisterType(
            typeof(IRepository&lt;&gt;),
            typeof(ValidatingRepository&lt;&gt;),
            "validating")
        .RegisterType&lt;IValidator&lt;StockQuote&gt;, RandomStockQuoteValidator&gt;()
        .RegisterType&lt;ILogger, CompositeLogger&gt;(
            "composite<b>"<span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">,</span></b>
<b>            <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new InjectionConstructor(</span></b>
<b>                <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new ResolvedArrayParameter&lt;ILogger&gt;(</span></b>
<b>                    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">typeof(ILogger),</span></b>
<b>                    <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new ResolvedParameter&lt;ILogger&gt;("UI"))));</span></b>

  StocksTickerPresenter presenter
        = container.Resolve&lt;StocksTickerPresenter&gt;(
            new ParameterOverride(
                "repository",
                new ResolvedParameter&lt;IRepository&lt;StockQuote&gt;&gt;("validating"))
                    .OnType&lt;StocksTickerPresenter);

  Application.Run((Form)presenter.View);
}
```

![](images/note.gif)Note:&gt; The **ResolvedArrayParameter** works like any other **InjectionParameterValue** used to specify how to supply a constructor argument or a property value. In this case, the **ILogger[]** parameter will be injected with a two-element array (because the **ResolvedArrayParameter** is built with two arguments). The first element will be the result of resolving the **ILogger** interface without a name (mapped to the **ConsoleLogger** in the configuration file), indicated by the **typeof(ILogger) **argument (which is shorthand for **new** **ResolvedParameter&lt;ILogger&gt;()**), while the second element will be the result of resolving the **ILogger** interface with the "**UI**" name (again mapped in the configuration file), indicated by the explicit **new ResolvedParameter&lt;ILogger&gt;("UI")** expression. When using this kind of array injection, literal values can be specified as members of the array and as unnamed instances.
4. Update the call to the **Resolve** method to supply a new **PropertyOverride** object for the **“Logger”** property so that the logger with name **"composite"** is resolved and injected.
```other
using (IUnityContainer container = new UnityContainer())
{
  container.LoadConfiguration();

  container
        .RegisterType(
            typeof(IRepository&lt;&gt;),
            typeof(ValidatingRepository&lt;&gt;),
            "validating")
        .RegisterType&lt;IValidator&lt;StockQuote&gt;, RandomStockQuoteValidator&gt;()
        .RegisterType&lt;ILogger, CompositeLogger&gt;(
            "composite",
            new InjectionConstructor(
                new ResolvedArrayParameter&lt;ILogger&gt;(
                    typeof(ILogger),
                    new ResolvedParameter&lt;ILogger&gt;("UI"))));

  StocksTickerPresenter presenter
        = container.Resolve&lt;StocksTickerPresenter&gt;(
            new ParameterOverride(
                "repository",
                new ResolvedParameter&lt;IRepository&lt;StockQuote&gt;&gt;("validating"))
                    .OnType&lt;StocksTickerPresenter&gt;()<b><span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">,</span></b>
<b>                <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new PropertyOverride("Logger", </span></b>
<b>                <span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">new ResolvedParameter&lt;ILogger&gt;("composite"))</span>);</b>

  Application.Run((Form)presenter.View);
}
```

![](images/note.gif)Note:&gt; The **PropertyOverride** is not limited to a single type, so it will apply to all resolved objects with a **“Logger”** property.



### Running the Application ###
Launch and use the application. All entries will be logged to both the ui.log file (located in the StocksTicker\bin\Debug folder) and the console.  
To verify you have completed the lab correctly, you can use the solution provided in the Lab04\end\StocksTicker folder.  

