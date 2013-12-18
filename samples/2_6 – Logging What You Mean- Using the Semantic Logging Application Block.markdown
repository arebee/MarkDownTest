---
Source File Name: 50-SemanticLogging.docx
AssetID: 5d4e1111-1c4d-499c-819a-b97085e5ba23
Title: 6 – Logging What You Mean: Using the Semantic Logging Application Block
Order In ToC: 2
Output Filename: 2_6 – Logging What You Mean- Using the Semantic Logging Application Block.markdown
---

#### Markdown Test ####
# 6 – Logging What You Mean: Using the Semantic Logging Application Block #
----------

![](images/note.gif)#!155CharTopicSummary!#:&gt; 
Learn how to use semantic logging to write messages to multiple destinations, control the format of those messages, and filter what messages get written.

# Introduction #
Why do you need another logging block when the Logging Application Block already has a comprehensive set of features? You’ll find that you can do many of the same things with the Semantic Logging Application Block: you can write log messages to multiple destinations, you can control the format of your log messages, and you can filter what gets written to the log. You also use the Semantic Logging Application Block for the same reasons: collecting diagnostic information from your application, debugging, and troubleshooting. The answer why you might want to use the Semantic Logging Application Block lies with how the application writes messages to the logs.  
Using a traditional logging infrastructure, such as that provided by the Logging Application Block, you might record an event entry of your application as shown in the following code sample:  

```csharp
LogEntry logEntry = new LogEntry();
logEntry.EventId = 100;
logEntry.Severity = TraceEventType.Information;
logEntry.Message = String.Format(
  @"Scaling request for role {0} has being successfully submitted.
The requested role instance count is {1}. 
The scaling operation was triggered by a rule named '{2}'.
The current role instance count is {3}.",
  request.RoleName, 
  request.InstanceCount,
  context.RuleName,
  context.CurrentInstanceCount);

logEntry.Categories.Add("Autoscaling Updates");
 
logWriter.Write(logEntry);
```

Using the Semantic Logging Application Block, you would record the same entry as shown in the following code sample:  

```csharp
MyCompanyEventSource.Log.ScalingRequestSubmitted(
  request.RoleName, 
  request.InstanceCount,
  context.RuleName,
  context.CurrentInstanceCount);
```

![](images/note.gif)#!SPOKENBY(Beth)!#:&gt; 
After using the Semantic Logging Application Block for a while, going back and using the Logging Application Block feels clumsy and unintuitive when writing application code.Notice how with the Semantic Logging Application Block you are simply reporting the fact that some event occurred that you might want to record in a log. You do not need to specify an event ID, a severity, or the format of the message when you create the event to log in your application code. This approach of using strongly typed events in your logging process provides the following benefits:  
+ You can be sure that you format and structure your log messages in a consistent way because there is no chance of making a coding error when you write the log message. For example, an event with a particular ID will always have the same verbosity, extra information, and payload structure.
+ It is easier to have a holistic view of all the application events, as they are defined in a centralized place, and refactor to make them consistent with each other.
+ It is easier to query and analyze your log files because the log messages are formatted and structured in a consistent manner.
+ You can more easily parse your log data using some kind of automation. This is especially true if you are using one of the Semantic Logging Application Block sinks that preserves the structure of the payload of the events; for example, the Windows Azure Table sink preserves the structure of your log messages. However, even if you are using flat files, you can be sure that the format of log messages with a particular ID will be consistent.
+ You can more easily consume the log data from another application. For example, in an application that automates activities in response to events that are recorded in a log file or a Windows Azure Table.
+ It is easier to correlate log entries from multiple sources.
![](images/note.gif)Note:&gt; The term <i>semantic logging</i> refers specifically to the use of strongly typed events and the consistent structure of the log messages in the Semantic Logging Application Block.While it is possible to use the **EventSource** class in the .NET framework (without the Semantic Logging Application Block) and  [Event Tracing for Windows](http://msdn.microsoft.com/en-us/library/windows/desktop/bb968803.aspx) (ETW) to write log messages from your application, using the Semantic Logging Application Block makes it easier to incorporate the functionality provided by the **EventSource** class, and makes it easier to manage the logging behavior of your application. The Semantic Logging Application Block makes it possible to write log messages to multiple destinations, such as flat files or a database, and makes it easier to access the capabilities of the **EventSource** class and control what your application logs by setting filters and logging verbosity. If you are using the block in-process, it does not use the ETW infrastructure to handle log messages but it does have the same semantic approach to logging as ETW. If you are using the block out-of-process, then it uses the ETW infrastructure in Windows to pass the log messages from your application to the sinks that process and save the log messages.  
The Semantic Logging Application Block is intended to help you move from the traditional, predominantly unstructured or quasi-structured logging approach (such as that offered by the Logging Application Block) towards the semantic logging approach offered by ETW. With the in-process approach, you don’t have to buy in to the full ETW infrastructure but it does enable you to start using semantic logging. The Semantic Logging Application Block enables you to use the **EventSource** class and semantic log messages in your applications without moving away from the log formats you are familiar with (such as flat files and database tables). In the future, you can easily migrate to a complete ETW-based solution without modifying your application code: you continue to use the same custom Event Source class, but use ETW tooling to capture and process your log messages instead of using the Semantic Logging Application Block event listeners.  
![](images/note.gif)#!SPOKENBY(Jana)!#:&gt; You can think of the Semantic Logging Application Block as a stepping stone from a traditional logging approach (such as that taken by the Logging Application Block), to a modern, semantic approach as provided by ETW. Even if you never migrate to using the ETW infrastructure, there’s plenty of value in adopting a semantic approach to your logging.You can also use the Semantic Logging Application Block to create an out-of-process logger, an approach that utilizes the ETW infrastructure in Windows. The key advantages of the out-of-process approach are that it makes logging more resilient to application crashes and facilitates correlating log data from multiple sources. If your application crashes, any log messages are likely to be already buffered in the ETW infrastructure and so will be processed and saved. This contrasts with the in-process approach where log messages are buffered and processed within your own application.   
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
Collecting trace messages from your production system in a separate, out-of-process, application helps to improve the resiliency of your logging processes. Many of the features of the Semantic Logging Application Block are similar to those of the Logging Application Block, and this chapter will refer to Chapter 5, “[As Easy As Falling Off a Log](test-markdown_6e1aeee5-feb6-4daa-9810-3dc6ca8700e9.html),” where appropriate.  


# What Does the Semantic Logging Application Block Do? #
When used in-process, the Semantic Logging Application Block enables you to use the **EventSource** class to write log messages from your application. The Semantic Logging Application Block receives notifications whenever the application writes a message using an **EventSource** class. The Semantic Logging Application Block then writes the message to one or more destinations of your choice. The Semantic Logging Application Block includes event sinks that can send log messages to a flat file, a console window, a database, or Windows Azure storage.  
Figure 1 illustrates how your application uses the Semantic Logging Application Block in-process. It uses a custom class that extends the **EventSource** class in the **System.Diagnostics.Tracing** namespace to enable you to write application-specific log messages. The event source then notifies the event listener in your application when there is a log message to handle, and then the event sinks, that you attach to the listener, save the log message. The event sinks write the log message to a destination such as file, a database table, or a Windows Azure storage table. All of this takes place in process using managed code.  
<p class="label" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><div class="caption">Figure 1 - The Semantic Logging Application Block In-Process Scenario</div></p>![](images/B0ED8E01EAFDAD9BFC09BAF937112D53.png)  
 Typically, you have a single custom event source class, but you can have more if needed. Depending on how you choose to attach the sinks to the listeners, it’s possible that you will have one event listener per sink.  
Figure 2 illustrates how you can use the Semantic Logging Application Block out-of-process. This is more complex to configure, but has the advantage of improving the resiliency of logging in your LOB application. This approach uses managed code in the process that sends the log messages and in the process that collects and processes the log messages; some unmanaged code from the operating system is responsible for delivering the log messages between the processes.  
![](images/note.gif)Note:&gt; In the out-of-process scenario, both the LOB application that generates log messages and the logging application that collects the messages must run on the same machine.<p class="label" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><div class="caption">Figure 2- Using Semantic Logging Application Block out-of-process</div></p>![](images/C77AC7756C6029E5C154328FA5101EC9.png)  
![](images/note.gif)#!SPOKENBY(Jana)!#:&gt; 
The Semantic Logging Application Block can work in-process or out-of-process: you can host the event listeners and sinks in your LOB application process or in a separate logging process. Either way you can use the same event sink and formatter types.

## In-Process or Out-of-Process? ##
When should you use the Semantic Logging Application Block in-process and when should you use it out-of-process? Although it is easier to configure and use the block in-process, there are a number of advantages to using it in the out-of-process scenario.  
Using the block out-of-process minimizes the risk of losing log messages if your line-of-business application crashes. When the line-of-business application creates a log message it immediately delivers it to the ETW infrastructure in the operating system, so that any log messages written by the line-of-business application will not get lost if that application crashes.  
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
You could still lose log messages in the out-of-process scenario if the server itself fails between the time the application writes the log message and the time the out-of-process host persists the message, or if the out-of-process host application crashes before it persists the message. However, out-of-process host is a robust and relatively simple application and is unlikely to crash. 
Notice, with very high throughput, you may lose messages if either the ETW or sink buffers become full.One further advantage of the out-of-process approach is that it makes it much easier for an administrator to change the configuration at run time without needing to update the LOB application.  


## Buffering Log Messages ##
Some sinks, such as the **WindowsAzureTableSink** and **SqlDatabaseSink** classes can buffer log messages for a configurable period of time (ten seconds by default). These sinks typically communicate over the network with the service that is ultimately responsible for persisting the log messages from your application. The block uses buffering in these sinks to improve performance: typically, chunky rather chatty communication over a network improves the overall throughput.  
However, using buffering introduces a trade-off: if the process that is buffering the messages crashes before delivering those log messages, then you lose those messages. The shorter the buffering period, the fewer messages you will lose in the event of an application crash, but at the cost of a lower throughput of log messages.  
One of the reasons for using the Semantic Logging Application Block out-of-process is to mitigate this risk. If the message buffer is in a separate process from your line-of-business application, then the messages are not lost in the event that the application crashes. The out-of-process host applications for Semantic Logging Application Block sinks are designed to be robust in order to minimize the chances of these applications crashing.  
You should monitor the log messages generated by the Semantic Logging Application Block for any indication that the have buffers overflowed and that you have lost messages. For example, log messages with event ids 900 and 901 indicate that a sink's internal buffers have overflowed; in the out-of-process scenario, event ids 806 and 807 indicate that the ETW buffers have overflowed. You can modify the buffering configuration options for the sinks to reduce the chance that the buffers overflow with your typical workloads.  
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
If you try to push a very high volume of events (in the order of magnitude of 100s or 1000s per second using the Windows Azure sink), you may lose some log messages.For more information, see the topic [Performance Considerations](http://go.microsoft.com/fwlink/p/?LinkID=304194) in the Enterprise Library Reference Documentation.  


# How Do I Use the Semantic Logging Application Block? #
It’s time to see some examples of the Semantic Logging Application Block in use, including how to create an event source, how to configure the block, and how to write log entries. The code samples included in this section are taken from the sample application (Semantic Logging) that accompanies this chapter: in some cases, the code is shown here differs slightly from the code in the sample to make it easier to read.  
<a name="CreatinganEventSource" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Creating an Event Source ##
Before you can write log messages using an **EventSource** class, you must define what log messages you will write. This is what semantic logging means. You can specify the log messages you will write by extending the **EventSource** class in the **System.Diagnostics.Tracing** namespace in the .NET 4.5 framework. In the terms used by ETW, an **EventSource** implementation represents an “Event Provider.”  
Each event type in your application is represented by a method in your **EventSource** implementation. These event methods take parameters that define the payload of the event and are decorated with attributes that provide additional metadata such as the event ID or verbosity level.  
![](images/note.gif)#!SPOKENBY(Carlos)!#:&gt; 
Behind the scenes, the event source infrastructure extracts information about your application’s events to build event schemas and a manifest by using reflection on your event source classes.The following code sample shows an example **EventSource** implementation. This example also includes the optional nested classes **Keywords** and **Tasks** that enable you to define additional information for your log messages.  

```csharp
[EventSource(Name = "MyCompany")]
public class MyCompanyEventSource : EventSource
{
    public class Keywords
    {
        public const EventKeywords Page = (EventKeywords)1;
        public const EventKeywords DataBase = (EventKeywords)2;
        public const EventKeywords Diagnostic = (EventKeywords)4;
        public const EventKeywords Perf = (EventKeywords)8;
    }
 
    public class Tasks
    {
        public const EventTask Page = (EventTask)1;
        public const EventTask DBQuery = (EventTask)2;
    }

    private static MyCompanyEventSource _log = new MyCompanyEventSource();
    private MyCompanyEventSource() { }
    public static MyCompanyEventSource Log { get { return _log; } }
 
    [Event(1, Message = "Application Failure: {0}", 
    Level = EventLevel.Critical, Keywords = Keywords.Diagnostic)]
    internal void Failure(string message)
    {
      this.WriteEvent(1, message);
    }
 
    [Event(2, Message = "Starting up.", Keywords = Keywords.Perf,
    Level = EventLevel.Informational)]
    internal void Startup()
    {
      this.WriteEvent(2);
    }
 
    [Event(3, Message = "loading page {1} activityID={0}",
    Opcode = EventOpcode.Start,
    Task = Tasks.Page, Keywords = Keywords.Page,
    Level = EventLevel.Informational)]
    internal void PageStart(int ID, string url)
    {
      if (this.IsEnabled()) this.WriteEvent(3, ID, url);
    }


    ...
}
```

The class, **MyCompanyEventSource** that extends the **EventSource** class, contains definitions for all of the events that you want to be able to log using ETW; each event is defined by its own method. You can use the **EventSource** attribute to provide a more user friendly name for this event source that ETW will use when you save log messages.  
![](images/note.gif)#!SPOKENBY(Jana)!#:&gt; 
This **EventSource** follows the recommended naming convention. The name should start with your company name, and if you expect to have more than one **event source** for your company the name should include categories separated by '-'. Microsoft follows this convention; for example, there is an event source called “Microsoft-Windows-DotNetRuntime.” You should carefully consider this name because if you change it in the future, users consuming the events will no longer receive them.Each event is defined using a method that wraps a call to the **WriteEvent** method in the **EventSource** class. The first parameter of the **WriteEvent** method is an event id that must be unique to that event, and different overloaded versions of this method enable you to write additional information to the log. Attributes on each event method further define the characteristics of the event. Notice that the **Event** attribute includes the same value as its first parameter.  
![](images/note.gif)#!SPOKENBY(Markus)!#:&gt; The custom event source file can get quite large; you should consider using partial classes to make it more manageable.In the **MyCompanyEventSource** class, the methods such as **PageStart** include a call to the **IsEnabled** method in the parent class to determine whether to write a log message. The **IsEnabled** method helps to improve performance if you are using the costly overload of the **WriteMethod** that includes a **params object[]** parameter.  
![](images/note.gif)#!SPOKENBY(Carlos)!#:&gt; You can also include the call to the **IsEnabled** method in a non-event method in your event source class that performs some pre-processing, transforming, or formatting of data before calling an event method. There is also an overload of the **IsEnabled** method that can check whether the event source is enabled for particular keywords and levels. However, with this overload, it’s important to make sure that the parameters to the **IsEnabled** method match the keywords and levels in the **EventAttribute** attribute. Checking that the parameters match is one of the checks that the **EventSourceAnalyzer** performs: this class is described later in this chapter.  
You can use the **Event** attribute to further refine the characteristics of specific events.  
For more information about the **EventSource** class and the **WriteEvent** and **IsEnabled** methods, see [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) on MSDN.  
The **MyCompanyEventSource** class also includes a static member called **Log** that provides access to a shared instance of the **MyCompanyEventSource** class, and defines the constructor as private. The following code sample shows an alternative approach for defining the access to the singleton.  

```csharp
private static readonly Lazy&lt;MyCompanyEventSource&gt; Instance =
  new Lazy&lt;MyCompanyEventSource&gt;(() =&gt; new MyCompanyEventSource());
 
private MyCompanyEventSource() { }
 
public static MyCompanyEventSource Log { get { return Instance.Value; } }
```



### Specifying the Event and its Payload ###
The event methods in your event source class enable you to provide discrete pieces of information about the event to include in the payload. For example, the **PageStart** event method enables you to include a URL in the payload. Some sinks, will store payload items individually; for example, the Windows Azure Table storage sink uses a separate column for each payload item. Payload items don’t necessarily have to be strings.  
The Event attribute can contain additional metadata parameters to further refine the output, and every parameter in this attribute is optional. The only required value is the event ID.  
![](images/note.gif)#!SPOKENBY(Carlos)!#:&gt; Developers in the team typically create a method in the custom event source class with the payload items and the bare minimum relevant metadata if there is any, and get back to coding the business logic. At a later point, someone on the team looks at the event source class as a whole and starts assigning the appropriate metadata to make the output of the entries consistent with each other.An event’s message is a human readable version of the payload information whose format you can control using the **Event** attribute’s **Message** parameter. The **Startup** method always writes the string “Starting up” to the log when you invoke it. The **PageStart** method writes a message to the log, substituting the values of the **ID** and **url** parameters for the two placeholders in the string "loading page {1} activityID={0}."  
![](images/note.gif)Note:&gt; Although the event source class does a lot of the heavy lifting for you, you still need to do your part. For example, you must also pass these parameters on to the call to the **WriteEvent** method, and ensure that you pass them in the same order as you define them in the method signature. However, this is one of the checks that the **EventSourceAnalyzer** class can perform for you. Usage of this class is described later in this chapter.

### Specifying the Log Level ###
You can use the **Level** parameter of the **Event** attribute to specify the severity level of the message. The **EventLevel** enumeration determines the available log levels: **Verbose (5)**, **Informational (4)**, **Warning (3)**, **Error (2)**, **Critical (1)**, and **LogAlways (0)**. **Informational** is the default logging level when not specified. When you enable an event source in your application, you can specify a log level, and the event source will log all log messages with same or lower log level. For example, if you enable an event source with the warning log level, all log methods with a level parameter value of **Warning**, **Error**, **Critical**, and **LogAlways** will be able to write log messages.  
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
You should use log levels less than **Informational** for relatively rare warnings or errors. When in doubt stick with the default of **Informational** level, and use the **Verbose** level for events that can happen more than 1,000 times per second. Typically, users filter by severity level first and then, if necessary, refine the filter using keywords. 

### Using Keywords ###
The example includes a **Keywords** parameter for the **Event** attribute for many of the log methods. You can use the optional keywords to define different groups of events so that when you enable an event source, you can specify which groups of events to enable: only events whose **Keywords** parameter matches one of the specified groups will be able to write to the log. You can also use the keywords to filter and analyze the events in your log.  
If you decide to use keywords, you must define the keywords you will use in a nested class called **Keywords** as shown in the example. Each keyword value is a 64-bit integer, which is treated as a bit array enabling you to define up to 64 different keywords. You can associate a log method with multiple keywords as shown in the following example where the **Failure** message is associated with both the **Diagnostic** and **Perf** keywords.  
![](images/note.gif)#!SPOKENBY(Markus)!#:&gt; 
Although **Keywords** looks like an enumeration, it’s a static class with constants of type **System.Diagnostics.Tracing.EventKeywords**. But just as with flags, you need to make sure you assign powers of two as the value for each constant.
```csharp
[Event(1, Message = "Application Failure: {0}", Level = EventLevel.Critical,
 Keywords = Keywords.Diagnostic|Keywords.Perf)]
internal void Failure(string message)
{
  if (this.IsEnabled()) this.WriteEvent(1, message); 
}
```

The following list offers some recommendations for using keywords in your organization.  
+ Events that you expect to fire less than 100 times per second do not need special treatment. You should use a default keyword for these events.
+ Events that you expect to fire more than 1000 times per second should have a keyword. This will enable you to turn them off if you don’t need them.
+ It’s up to you to decide whether events that typically fire at a frequency between these two values should have a keyword.
+ Users will typically want to switch on a specific keyword when they enable an event source, or enable all keywords.
+ Even when the frequency of events is not high, you might want to use keywords to be able to analyze the log and filter messages at a later point.
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
Keep it simple for users to enable just the events they need.

### Using Opcodes and Tasks ###
You can use the **Opcodes** and **Tasks** parameters of the **Event** attribute to add additional information to the message that the event source logs. The **Opcodes** and **Tasks** are defined using nested classes of the same name in a similar way to how you define **Keywords**: **Opcodes** and **Tasks** don’t need to be assigned values that are powers of two. The example event source includes two tasks: **Page** and **DBQuery**.   
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; The logs contain just the numeric task and opcode identifiers. The developers who write the **EventSource** class and IT Pros who use the logs must agree on the definitions of the tasks and opcodes used in the application. Notice how in the sample, the task constants have meaningful names such as **Page** and **DBQuery**: these tasks appear in the logs as task ids **1** and **2** respectively. 
![](images/note.gif)Note:&gt; If you choose to define custom opcodes, you should assign integer values of 11 or above, otherwise they will clash with the opcodes defined in the **EventOpcode** enumeration. If you define a custom opcode with a value of 10 or below, messages that use these opcodes will not be delivered.

### Sensitive Data ###
You should make sure that you do not write sensitive data to the logs where it may be available to someone who shouldn’t have access to that data. One possible approach is to scrub sensitive data from the event in the **EventSource** class itself. For example, if you had a requirement to write details of a connection string in a log event, you could remove the password using the following technique.  

```csharp
[Event(250, Level = EventLevel.Informational,
 Keywords = Keywords.DataAccess, Task = Tasks.Initialize)]
public void ExpenseRepositoryInitialized(string connectionString)
{
  if (this.IsEnabled(EventLevel.Informational, Keywords.DataAccess))
  {
    // Remove sensitive data
    var csb = new SqlConnectionStringBuilder(connectionString)
      { Password = string.Empty };
    this.WriteEvent(250, csb.ConnectionString);
  }
}
```



### Verifying your EventSource Class ###
As you’ve seen, there are a number of conventions that you must follow when you author a custom **EventSource** class, such as matching the id in the **Event** attribute to the id passed to the **WriteEvent** method that aren’t checked by the standard tools in Visual Studio or by the **EventSource** class at run time. Although the **EventSource** class does perform some basic checks, such as that the supplied event ID is valid, these checks are not exhaustive. Using the Semantic Logging Application Block, you can check that your custom event source for such common errors as part of your unit testing. The Semantic Logging Application Block includes a helper class for this purpose named **EventSourceAnalyzer**. The **Inspect** method checks your **EventSource** class as shown in the following code sample.  

```csharp
[TestMethod]
public void ShouldValidateEventSource()
{
    EventSourceAnalyzer.InspectAll(SemanticLoggingEventSource.Log);
}
```

If your Visual Studio solution does not include test projects, you can still use the **Inspect** method to check any custom **EventSource** class as shown in the sample code that accompanies this guide.  
The **EventSourceAnalyzer** class checks your custom **EventSource** class using the following techniques.  
+ It attempts to enable a listener using the custom **EventSource** class to check for basic problems.
+ It attempts to retrieve the event schemas by generating a manifest from the custom **EventSource** class.
+ It attempts to invoke each event method in the custom **EventSource** class to check that it returns without error, supplies the correct event ID, and that all the payload parameters are passed in the correct order.
For more information about the **EventSourceAnalyzer** class, see the topic [Checking an EventSource Class for Errors](http://go.microsoft.com/fwlink/p/?LinkID=304195)  in the Enterprise Library Reference Documentation.  


## Versioning your EventSource Class ##
The methods in your custom **EventSource** class will be called from multiple places in your application, and possibly from multiple applications if you share the same **EventSource** class. You should take care when you modify your **EventSource** class, that any changes you make do not have unexpected consequences for your existing applications. If you do need to modify your **EventSource** class, you should restrict your changes to adding methods to support new log messages, and adding overloads of existing methods (that would have a new event ID). You should not delete or change the signature of existing methods.  
This is especially important in light of the fact that if you have multiple versions of your **EventSource** class in multiple applications that are using the Semantic Logging Application Block out-of-process approach, then the version of the event schema that the host service uses will not be predictable. You should ensure that you have procedures in place to synchronize any changes to the **EventSource** class across all of the applications that share it.  


## Adding the Semantic Logging Application Block to Your Project ##
Before you write any code that uses the Semantic Logging Application Block, you must install the required assemblies to your project. You can install the block by using the NuGet package manager in Visual Studio: in the **Manage Nuget Packages** dialog, search online for the **EnterpriseLibrary.SemanticLogging** package and install it. If you plan to use the database sinks you also need to add the **EnterpriseLibrary.SemanticLogging.Database** NuGet package. If you plan to use the Windows Azure Table storage sink, you also need to add the **EnterpriseLibrary.SemanticLogging.WindowsAzure** NuGet package.  
![](images/note.gif)#!SPOKENBY(Carlos)!#:&gt; You don’t need to add a reference to the Semantic Logging Application Block to your application if you use the block out-of-process.

## Configuring the Semantic Logging Application Block ##
How you configure the Semantic Logging Application Block depends on whether you are using it in-process or out-of-process. If you are using the block in-process, then you provide the configuration information for your sinks in code; if you are using the block out-of-process, then you provide the configuration information in an XML file. The section “<a href="#HowDoIUsetheSemantic" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">How do I Use the Semantic Logging Application Block to Log Trace Messages Out-of-Process?</a>” later in this chapter describes the configuration in the out-of-process scenario. This section describes the in-process scenario.  
Typically, you create an **ObservableEventListener** instance to receive the events from your application, then you create any sinks you need to save the log messages. The built-in sinks have constructors that enable you to configure the sink as you create it. However, the block includes convenience methods, such as **LogToConsole** and **LogToSqlDatabase**, to set up the sinks and attach them to a listener. Once the listener is created, you enable one or more event sources to listen to, as well as the highest level of event to capture, and any keywords to filter on for each source. The following code sample shows an example that creates, configures, and enables two sinks, one to write log messages to the console and one to write log messages to a SQL Database.  

```csharp
// Initialize the listeners and sinks during application start-up
var listener1 = new ObservableEventListener();

listener1.EnableEvents(
  MyCompanyEventSource.Log, EventLevel.LogAlways, 
  MyCompanyEventSource.Keywords.Perf | MyCompanyEventSource.Keywords.Diagnostic);

listener1.LogToConsole();


var listener2 = new ObservableEventListener();

listener2.EnableEvents(
  MyCompanyEventSource.Log, EventLevel.LogAlways, Keywords.All);

// The SinkSubscription is used later to flush the buffer, 
// although this is typically not needed for basic scenarios
SinkSubscription&lt;SqlDatabaseSink&gt; subscription =
  listener2.LogToSqlDatabase("Demo Semantic Logging Instance",
  connectionString);
```

This example uses two listeners because it is using different keyword filters for each one.  
![](images/note.gif)#!SPOKENBY(Jana)!#:&gt; 
You should realize that creating an instance of an observable event listener results in shared state. You should set up your listeners at bootstrap time and dispose of them when you shut down your application.An alternative approach is to use the static entry points provided by the log classes as shown in the following code sample. The advantage of this approach is you only need to invoke a single method rather than creating the listener and attaching the sink. However, this method does not give you access to the sink and therefore you cannot explicitly flush any buffers associated with the sink when you are shutting down the application.  
![](images/note.gif)Note:&gt; You can use the **onCompletedTimeout** parameter to the **CreateListener** method controls how long a listener will wait for the sink to flush itself before disposing the sinks. For more information, see the topic [Event Sink Properties](http://go.microsoft.com/fwlink/p/?LinkID=304196) in the Enterprise Library Reference Documentation.
```csharp
// Create the event listener using the static method.
// Typically done when the application starts.
var listener = ConsoleLog.CreateListener();
listener.EnableEvents(MyCompanyEventSource.Log, EventLevel.LogAlways,
  Keywords.All);
 
...
 
// Disable and dispose the event listener.
// Typically done when the application terminates.
listener.DisableEvents(MyCompanyEventSource.Log);
listener.Dispose();
```

The console and file based sinks can also use a formatter to control the format of the output. These formatters may also have configuration options. The following code sample shows an example that configures a JSON formatter for the console sink, and uses a custom console color mapper.  

```csharp
JsonEventTextFormatter formatter =
  new JsonEventTextFormatter(EventTextFormatting.Indented);

var colorMapper = new MyCustomColorMapper();
listener1.LogToConsole(formatter, colorMapper);
listener1.EnableEvents(
  MyCompanyEventSource.Log, EventLevel.LogAlways, Keywords.All);
```

A color mapper is a simple class that specifies the color to use for different event levels as shown in the following code sample.  

```csharp
public class MyCustomColorMapper : IConsoleColorMapper
{
  public ConsoleColor? Map(
    System.Diagnostics.Tracing.EventLevel eventLevel)
  {
    switch (eventLevel)
    {
      case EventLevel.Critical:
        return ConsoleColor.White;
      case EventLevel.Error:
        return ConsoleColor.DarkMagenta;
      case EventLevel.Warning:
        return ConsoleColor.DarkYellow;
      case EventLevel.Verbose:
        return ConsoleColor.Blue;
      default:
        return null;
    }
  }
```



## Writing to the Log ##
Before you can write a log message, you must have an event source class in your application that defines what log messages you can write. The section “<a href="#CreatinganEventSource" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Creating an Event Source</a>” earlier in this chapter describes how you can define such an event source. You must also add the Semantic Logging Application Block to your application: the easiest way to do this is using NuGet.  
The following code sample shows a simple example of how you can use the event source shown previously in this chapter to write messages to a console window.  

```csharp
class Program
{
  static void Main(string[] args)
  {
    // Create the event listener
    var listener = new ObservableEventListener();
    listener.EnableEvents(MyCompanyEventSource.Log, EventLevel.LogAlways,
      Keywords.All);
    listener.LogToConsole();

    MyCompanyEventSource.Log.StartUp();

    // Application code goes here
    ...

    listener.DisableEvents(MyCompanyEventSource.Log);
    listener.Dispose();
     }
}
```

This sample shows how to create an instance of the **ObservableEventListener** class from the Semantic Logging Application Block, enable the listener to process log messages from the **MyCompanyEventSource** class, write a log message, disable the listener, and then dispose of the listener. Typically, you will create and enable an event listener when your application starts up and initializes, and disable and dispose of your event listener as the application shuts down.  
![](images/note.gif)#!SPOKENBY(Jana)!#:&gt; 
For those sinks that buffer log messages, such as the Windows Azure Table storage sink, you can flush the sink by calling the **FlushAsync** method to ensure that all events are written. Nevertheless, the sink will also flush automatically when disposing the listener. Keep in mind that the **Dispose** call will block the thread until the events are persisted, but you can also specify a timeout when initializing the sink.When you enable an event listener for an event source, you can specify which level of events from that event source should be logged and which groups of events (identified using the keywords) should be active. The example shows how to activate log methods at all levels and with all keywords.  
![](images/note.gif)Note:&gt; If you want to activate events in all groups, you must use the **Keywords.All** parameter value. If you do not supply a value for this optional parameter, only events with no keywords are active.The following code sample shows how to activate log messages with a level of **Warning** or lower with keywords of either **Perf** or **Diagnostic**.  

```csharp
listener.EnableEvents(MyCompanyEventSource.Log, EventLevel.Warning,
  MyCompanyEventSource.Keywords.Perf | MyCompanyEventSource.Keywords.Diagnostic);
```

<a name="HowDoIUsetheSemantic" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# How do I Use the Semantic Logging Application Block to Log Events Out-of-Process? #
To use the Semantic Logging Application Block to process events out-of-process you must add the necessary code to your LOB application to create trace messages when interesting events occur in the application in the same way that you do in-process, by creating a class derived from **EventSource**. You must also run and configure a separate process that is responsible for collecting and processing the events.  
The LOB application itself will use the built-in **EventSource** infrastructure to write events through ETW, and will not require a reference to the Semantic Logging Application Block.  
![](images/note.gif)#!SPOKENBY(Jana)!#:&gt; 
You should consider collecting and processing trace messages in a separate process to improve the robustness of your logging solution. If your LOB crashes, a separate process will be responsible for any logging. 
You should also consider collecting and processing events in a separate process if you are using a sink with a high latency such as the Windows Azure Table storage sink and you want the buffering to not prevent the application from shutting down very quickly.

## Running the Out-of-Process Host Application ##
The Semantic Logging Application Block includes the SemanticLogging-svc host application for you to use; you can run this application as a Windows Service or as a console application.  
Typically, you should run the Out-of-Process Host as a console application when you are developing and testing your logging behavior. It’s convenient to be able to stop and start the event listener host application and view any log messages in a console window.  
In a production environment, you should run the Out-of-Process Host as a Windows Service. You can easily configure a Windows Service to start when the operating system starts. In a production environment, you are unlikely to want to see log messages as they are processed in a console window: more likely, you will want to save the log messages to a file, database, or some other persistent storage.  
If you plan to use an out-of-process host in Windows Azure, you should run it as a Windows Service. You can install and start the service in a Windows Azure start up task.   
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
As an alternative to the Out-of-Process Host application included with the block, you could configure ETW to save the trace messages to a .etl file and use another tool to read this log file. However, the Out-of-Process Host application offers more choices of where to persist the log messages.By default, the Out-of-Process Host application reads the configuration for the block from a file named SemanticLogging-svc.xml. It reads this file at start up, and then continues to monitor this file for changes. If it detects any changes, it dynamically reconfigures the block based in the changes it discovers. If you make any changes to the **traceEventService** element, this will cause the service to recycle; for any changes related to sink definitions, the block will reconfigure itself without recycling the service. If the block detects any errors when it loads the new configuration, it will log an error in the Windows Event Log and continue to process the remainder of the configuration file. You should check the messages in the Windows Event Log to verify that your changes were successful.  
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
There may be a limit on the number of ETW sessions that you can create on a machine dependent on the resources available on that machine. The block will log an error if it can’t create sessions for all of the sinks defined in your SemanticLogging-svc.xml file.

### Updating an Event Source Class in the Out-of-Process Scenario ###
You should be careful if you use the same custom **EventSource** class in multiple applications. It’s possible in the out-of-process scenario that you might modify this **EventSource** class in one of the applications that’s creating event messages, and if you don’t change the event source name or id using the attributes in the event source class, the changes will be picked up by the out-of-process host without it restarting. The most recent changes to the custom **EventSource** class will take precedence and be used by the out-of-process host, which may have an impact on the logging behavior of the other applications. Ideally, you should keep your event source class in sync across any applications that share it.  


## Creating Trace Messages ##
You create trace messages in the LOB application in exactly the same way as described previously in this chapter. First, create a custom **EventSource** class that defines all of the events your application uses as shown in the following code sample.  

```csharp
[EventSource(Name = "MyCompany")]
public class MyCompanyEventSource : EventSource
{
    ...
 
    [Event(1, Message = "Application Failure: {0}", 
    Level = EventLevel.Critical, Keywords = Keywords.Diagnostic)]
    internal void Failure(string message)
    {
      if (this.IsEnabled()) this.WriteEvent(1, message);
    }
 
    ...
 
    public static readonly MyCompanyEventSource Log = new MyCompanyEventSource();
}
```

![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
Each **EventSource** implementation should have a unique name, although you might choose to reuse an **EventSource** in multiple applications. If you use the same name for the event source in multiple applications, all of the events from those applications will be collected and processed by a single logging application. If you omit the **EventSource** attribute, ETW uses the class name as the name of the event source.Second, create the trace messages in your application code as shown in the following code sample.  

```csharp
MyCompanyEventSource.Log.CouldNotConnectToServer();
```

You don’t need to create any event listeners or sinks in the LOB application if you are processing the log messages in a separate application.  


## Choosing Sinks ##
The Semantic Logging Application Block includes sinks that enable you to save log messages to the following locations: a database, a Windows Azure table, and to flat files. The choice of which sinks to use depends on how you plan to analyze and monitor the information you are collecting. For example, if you save your log messages to a database you can use SQL to query and analyze the data. If you are collecting log messages in a Windows Azure application you should consider writing the messages to Windows Azure table storage. You can export data from Windows Azure Table storage to a local file for further analysis on-premises.  
![](images/note.gif)#!SPOKENBY(Carlos)!#:&gt; 
Bear in mind that single-row-based formats (such as CSV) may not be best for exporting your entries since the messages and payload items could span several lines. XML files may be a better choice.
The console sink is useful during development and testing because you can easily see the trace messages as they appear, however the console sink is not appropriate for use in a production environment.  
![](images/note.gif)Note:&gt; If you use the **XmlEventTextFormatter** class to save the log messages formatted as XML in a flat file, you will need to perform some post-processing on the log files before you can use a tool such as Log Parser to read them. This is because the flat file does not have an XML root element, therefore the file does not contain well-formed XML. It’s easy to add opening and closing tags for a root element to the start and end of the file.
Although the block does not include a CSV formatter, you can easily export log messages in CSV format from Windows Azure Table storage or a SQL Server database.
The Log Parser tool can add a header to some file formats such as CSV, but it cannot add a header or a footer to an XML file.

## Collecting and Processing the Log Messages ##
To collect and process the log messages from the LOB application you run the Enterprise Library Semantic Logging Out-of-Process Windows Service/Console Host (SemanticLogging-svc.exe) included with the Semantic Logging Application Block. You can run this application as a console application or install it as a Windows service. This application uses configuration information from an XML file to determine which event sources to collect trace messages from and which sinks to use to process those messages.  
The default name for the configuration file is SemanticLogging-svc.xml. When you edit this XML file in Visual Studio, you can use the supplied schema file (SemanticLogging-svc.xsd) to provide IntelliSense support in the editor.  
The following snippet shows an example configuration file that defines how to collect trace messages written by another application using the Adatum event source. This example uses three sinks to write log messages to three different destinations. Each sink is configured to log messages with different severity levels. Additionally, the console sink filters for messages that have the **Diagnostics** or **Perf** keywords (The **Diagnostics** constant has a value of four and the **Perf** constant has a value of eight in the nested **Keywords** class in the **AdatumEventSource** class).  

```other
&lt;?xml version="1.0"?&gt;
&lt;configuration
  xmlns=http://schemas.microsoft.com/practices/2013/entlib/semanticlogging/etw
  xmlns:xsi=http://www.w3.org/2001/XMLSchema-instance
  xsi:schemaLocation="http://schemas.microsoft.com/practices/2013/entlib/
  semanticlogging/etw SemanticLogging-svc.xsd"&gt;
  &lt;!-- Optional settings for this host --&gt;
  &lt;traceEventService/&gt;
 
  &lt;!-- Event sink definitions used by this host to
  listen ETW events emitted by these EventSource instances --&gt;
  &lt;sinks&gt;
    &lt;consoleSink name="ConsoleEventSink"&gt;
      &lt;sources&gt;
        &lt;eventSource name="MyCompany" level="LogAlways" matchAnyKeyword="12"/&gt;
      &lt;/sources&gt;
      &lt;eventTextFormatter header="+=========================================+"/&gt;
    &lt;/consoleSink&gt;
    
    &lt;rollingFlatFileSink name="RollingFlatFileSink" 
                         fileName="RollingFlatFile.log" 
                         timeStampPattern="yyyy" 
                         rollFileExistsBehavior="Overwrite" 
                         rollInterval="Day"&gt;
      &lt;sources&gt;
        &lt;eventSource name="MyCompany"
                     level="Error"/&gt;
      &lt;/sources&gt;
    &lt;/rollingFlatFileSink&gt;
 
    &lt;sqlDatabaseSink name="SQL Database Sink"
                     instanceName="Demo"
                     connectionString="Data Source=(localDB)\v11.0;
                     Initial Catalog=Logging;
                     Integrated Security=True"&gt;      
      &lt;sources&gt;
        &lt;eventSource name="MyCompany"
                     level="Warning"/&gt;
      &lt;/sources&gt;
    &lt;/sqlDatabaseSink&gt;
  &lt;/sinks&gt;
 
&lt;/configuration&gt;
```

If you are collecting high volumes of trace messages from a production application, you may need to tweak the settings for the trace event service in the XML configuration file. For information about these settings, see the topic [Performance Considerations](http://go.microsoft.com/fwlink/p/?LinkID=304194) in the Enterprise Library Reference Documentation.  
A single instance of the console application or Windows service can collect messages from multiple event sources in multiple LOB applications. You can also run multiple instances, but each instance must use a unique session name.  
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
By default, each instance of the console application has a unique session name. However, if you decide to provide a session name in the XML configuration file as part of the trace event service settings, you must ensure that the name you choose is unique. 

# Customizing the Semantic Logging Application Block #
The Semantic Logging Application Block provides a number of extension points that enable you to further customize its behavior. These extension points include:  
+ Creating custom filters and manipulating events using Reactive Extensions.
+ Creating custom formatters for use both in-process and out-of-process. 
+ Creating custom sinks for use both in-process and out-of-process. 
+ Creating a custom application to collect trace messages from your LOB applications.
The example code in this section comes from the sample applications that are included with this guidance. In some cases, the code has been changed slightly to make it easier to read.  


## Creating Custom Filters Using Reactive Extensions (Rx) ##
The sink classes included with the Semantic Logging Application Block implement the **IObserver** interface, which enables them to subscribe to event listeners that implement the **IObservable** interface. By creating custom code that subscribes to an **IObservable** instance and that generates a new stream that an **IObserver** instance can subscribe to, you can manipulate the events before they are seen by the sink. This example illustrates how you can use Reactive Extensions to build a custom filter that controls which events are sent to the sinks in the in-process scenarios.  
![](images/note.gif)#!SPOKENBY(Markus)!#:&gt; 
The Semantic Logging Application Block does not have a dependency on Reactive Extensions (Rx), but this specific example does require you to add the Rx NuGet package to your project because it uses the **Observable** class and the utilities that the Rx library brings.This example shows how you can buffer some log messages until a specific event takes place. In this example, no informational messages are sent to the sink, until the listener receives an error message. When this happens, the listener the sends the last ten buffered informational messages and the error message to the sink. This is useful if, for the majority of the time you are not interested in the informational messages, but when an error occurs, you want to see the most recent informational messages that might have led to that error.  
The following code sample shows the **FlushOnTrigger** extension method that implements this behavior. It uses a class called **CircularBuffer** to hold the most recent messages, and the sample application includes a simple implementation of this **CircularBuffer** class.  

```csharp
public static IObservable&lt;T&gt; FlushOnTrigger&lt;T&gt;(
  this IObservable&lt;T&gt; stream, Func&lt;T, bool&gt; shouldFlush, int bufferSize)
{
  return Observable.Create&lt;T&gt;(observer =&gt;
  {
    var buffer = new CircularBuffer&lt;T&gt;(bufferSize);
    var subscription = stream.Subscribe(newItem =&gt;
      {
         if (shouldFlush(newItem))
         {
            foreach (var buffered in buffer.TakeAll())
            {
               observer.OnNext(buffered);
            }

            observer.OnNext(newItem);
         }
         else
         {
            buffer.Add(newItem);
         }
      },
      observer.OnError,
      observer.OnCompleted);

    return subscription;
  });
}
```

The following code sample shows how to use this method when you are initializing a sink. In this scenario, you must invoke the **FlushOnTrigger** extension method that creates the new stream of events for the sink to subscribe to.  

```csharp
var listener = new ObservableEventListener();
listener.EnableEvents(MyCompanyEventSource.Log, EventLevel.Informational,Keywords.All);

listener
  .FlushOnTrigger(entry =&gt; entry.Schema.Level &lt;= EventLevel.Error, bufferSize: 10)
  .LogToConsole();
```



## Creating Custom Formatters ##
The Semantic Logging Application Block includes three text formatters to format the log messages written to the console or to text files: **EventTextFormatter**, **JsonTextFormatter**, and **XmlTextFormatter**.  
There are three different scenarios to consider if you are planning to create a new custom event text formatter for the Semantic Logging Application Block. They are listed in order of increasing level of complexity to implement:  
+ Creating a custom formatter for use in-process.
+ Creating a custom formatter for use out-of-process without IntelliSense support when you edit the configuration file.
+ Creating a custom formatter for use out-of-process with IntelliSense support when you edit the configuration file.


### Creating a Custom In-Process Event Text Formatter ###
To create a custom text formatter, you create a class that implements the **IEventTextFormatter** interface and then use that class as your custom formatter. This interface contains a single method called **WriteEvent**.  
The following code sample shows how to create a custom text formatter similar to the built-in **EventTextFormatter**, but prefixes the properties of an event entry with a user supplied value.  

```csharp
public class PrefixEventTextFormatter : IEventTextFormatter
{
  public PrefixEventTextFormatter(string header, string footer,
    string prefix, string dateTimeFormat)
  {
    this.Header = header;
    this.Footer = footer;
    this.Prefix = prefix;
    this.DateTimeFormat = dateTimeFormat;
  }
 
  public string Header { get; set; }
  public string Footer { get; set; }
  public string Prefix { get; set; }
  public string DateTimeFormat { get; set; }
 
  public void WriteEvent(EventEntry eventEntry, TextWriter writer)
  {
    // Write header
    if (!string.IsNullOrWhiteSpace(this.Header))
      writer.WriteLine(this.Header);
 
    // Write properties
    writer.WriteLine("{0}SourceId : {1}",
      this.Prefix, eventEntry.ProviderId);
    writer.WriteLine("{0}EventId : {1}",
      this.Prefix, eventEntry.EventId);
    writer.WriteLine("{0}Keywords : {1}",
      this.Prefix, eventEntry.Schema.Keywords);
    writer.WriteLine("{0}Level : {1}",
      this.Prefix, eventEntry.Schema.Level);
    writer.WriteLine("{0}Message : {1}",
      this.Prefix, eventEntry.FormattedMessage);
    writer.WriteLine("{0}Opcode : {1}",
      this.Prefix, eventEntry.Schema.Opcode);
    writer.WriteLine("{0}Task : {1} {2}",
      this.Prefix, eventEntry.Schema.Task, eventEntry.Schema.TaskName);
    writer.WriteLine("{0}Version : {1}",
      this.Prefix, eventEntry.Schema.Version);
    writer.WriteLine("{0}Payload :{1}",
      this.Prefix, FormatPayload(eventEntry));
    writer.WriteLine("{0}Timestamp : {1}",
      this.Prefix, eventEntry.GetFormattedTimestamp(this.DateTimeFormat));
 
 
    // Write footer
    if (!string.IsNullOrWhiteSpace(this.Footer))
      writer.WriteLine(this.Footer);
 
    writer.WriteLine();
  }
 
  private static string FormatPayload(EventEntry entry)
  {
    var eventSchema = entry.Schema;
    var sb = new StringBuilder();
    for (int i = 0; i &lt; entry.Payload.Count; i++)
    {
      // Any errors will be handled in the sink
      sb.AppendFormat(" [{0} : {1}]", eventSchema.Payload[i], entry.Payload[i]);
    }
    return sb.ToString();
  }
}
```

You can use the **CustomFormatterUnhandledFault** method in the **SemanticLoggingEventSource** class to log any unhandled exceptions in your custom formatter class, but you should always avoid throwing an exception in the **WriteEvent** method body.  
Using text formatters when you are using the Semantic Logging Application Block in-process is a simple case of instantiating and configuring in code the formatter you want to use, and then passing it as a parameter to the method that initializes the sink that you’re using. The following code sample shows how to create and configure an instance of the **PrefixEventTextFormatter** formatter class and pass it to the **LogToConsole** extension method that subscribes **ConsoleSink** instance to the event listener:  

```csharp
var formatter = new PrefixEventTextFormatter("---", null, "&gt;&gt; ", "d");

listener.LogToConsole(formatter);
listener.EnableEvents(MyCompanyEventSource.Log, EventLevel.LogAlways, Keywords.All);
```



### Creating a Custom Out-of-Process Event Text Formatter without IntelliSense Support ###
Built-in formatters have IntelliSense support in the XML for configuring the Out-of-Process host, but custom formatters do not by default. You can use the custom event text formatter shown in the previous section in an out-of-process scenario by using the **customEventTextFormatter** element in the configuration file. The following XML sample shows how to use this built-in element.  

```csharp
&lt;sinks&gt;
  &lt;consoleSink name="Consolesink"&gt;
    &lt;sources&gt;
      &lt;eventSource name="MyCompany" level="LogAlways" /&gt;
    &lt;/sources&gt;

    &lt;customEventTextFormatter
      type="CustomTextFormatter.PrefixEventTextFormatter, CustomTextFormatter"&gt;
      &lt;parameters&gt;
        &lt;parameter name="header" type="System.String"
          value="=============================================="/&gt;
        &lt;parameter name="footer" type="System.String"
          value="=============================================="/&gt;
        &lt;parameter name="prefix" type="System.String" value="&gt; "/&gt;
        &lt;parameter name="dateTimeFormat" type="System.String" value="O"/&gt;
      &lt;/parameters&gt;
    &lt;/customEventTextFormatter&gt;
  &lt;/consoleSink&gt;
  ...
```

This example illustrates how you must specify the type of the custom formatter, and use **parameter** elements to define the configuration settings. The out-of-process host application looks scans the folder it runs from for DLLs that contain types that implement the **IEventTextFormatter** interface.  
You can use the same class that implements the IEventTextFormatter interface in both in-process and out-of-process scenarios.   
You must ensure that the order and type of the **parameter** elements in the configuration file match the order and type of the constructor parameters in the custom formatter class.  


### Creating a Custom Out-of-Process Event Text Formatter with IntelliSense Support ###
Instead of using the **customEventTextFormatter** and **parameter** elements in the XML configuration file, you can define your own custom element and attributes and enable IntelliSense support in the Visual Studio XML editor. In this scenario, the XML configuration file looks like the following sample:  

```other
&lt;?xml version="1.0"?&gt;
&lt;configuration
 xmlns=http://schemas.microsoft.com/practices/2013/entlib/semanticlogging/etw
 xmlns:xsi=http://www.w3.org/2001/XMLSchema-instance
 xsi:schemaLocation="urn:sample.etw.customformatter
 PrefixEventTextFormatterElement.xsd
 http://schemas.microsoft.com/practices/2013/entlib/semanticlogging/etw
 SemanticLogging-svc.xsd"&gt;
&lt;traceEventService /&gt;

&lt;sinks&gt;
  &lt;consoleSink name="Consolesink"&gt;
    &lt;sources&gt;
      &lt;eventSource name="MyCompany" level="LogAlways" /&gt;
    &lt;/sources&gt;
 
    &lt;prefixEventTextFormatter xmlns="urn:sample.etw.customformatter"
     header="==============================================================="
     footer="==============================================================="
     prefix="# "
     dateTimeFormat="O"/&gt;
  &lt;/consoleSink&gt;
  ...
```

![](images/note.gif)#!SPOKENBY(Markus)!#:&gt; 
Notice how the **prefixEventTextFormatter** element has a custom XML namespace and uses a **schemaLocation** to specify the location of the schema file.The custom XML namespace that enables IntelliSense behavior for the contents of the **prefixEventTextFormatter** element is defined in the XML schema file shown in the following sample.  

```other
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;xs:schema id="PrefixEventTextFormatterElement"
    targetNamespace="urn:sample.etw.customformatter"
    xmlns="urn:sample.etw.customformatter"
    xmlns:xs="http://www.w3.org/2001/XMLSchema"           
    elementFormDefault="qualified"
    attributeFormDefault="unqualified"&gt;
 
  &lt;xs:element name="prefixEventTextFormatter"&gt;
    &lt;xs:complexType&gt;
      &lt;xs:attribute name="header" type="xs:string" use="required" /&gt;
      &lt;xs:attribute name="footer" type="xs:string" use="required" /&gt;
      &lt;xs:attribute name="prefix" type="xs:string" use="required" /&gt;
      &lt;xs:attribute name="dateTimeFormat" type="xs:string" use="required" /&gt;
    &lt;/xs:complexType&gt;
  &lt;/xs:element&gt;
  
&lt;/xs:schema&gt;
```

You should place this schema file, along with the XML configuration file, in the folder where you are running the listener host application.  
In addition to creating the XML schema, you must also create an element definition that enables the block to read the configuration from the XML file and create an instance of the formatter. The following code sample shows the element definition class for this example.  

```csharp
public class PrefixEventTextFormatterElement : IFormatterElement
{
  private readonly XName formatterName = 
    XName.Get("prefixEventTextFormatter", "urn:sample.etw.customformatter");
 
  public bool CanCreateFormatter(System.Xml.Linq.XElement element)
  {
    return this.GetFormatterElement(element) != null;
  }
 
  public IEventTextFormatter CreateFormatter(System.Xml.Linq.XElement element)
  {
    var formatter = this.GetFormatterElement(element);
 
    var header = (string)formatter.Attribute("header");
    var footer = (string)formatter.Attribute("footer");
    var prefix = (string)formatter.Attribute("prefix");
    var datetimeFormat = (string)formatter.Attribute("dateTimeFormat");
 
    return new PrefixEventTextFormatter(header,footer, prefix, datetimeFormat);
  }
 
  private XElement GetFormatterElement(XElement element)
  {
    return element.Element(this.formatterName);
  }
}
```

This class implements the **IFormatterElement** interface that includes the methods **CanCreateFormatter** and **CreateFormatter**.  


## Creating Custom Sinks ##
The Semantic Logging Application Block includes a number of sinks for receiving and processing log messages such as the **ConsoleSink**, the **EventLogSink**, and the **RollingFlatFileSink**.  
There are three different scenarios to consider if you are planning to create a new custom sink for the Semantic Logging Application Block. They are listed in order of increasing level of complexity to implement:  
+ Creating a custom sink for use in-process.
+ Creating a custom sink for use out-of-process without IntelliSense support when you edit the configuration file.
+ Creating a custom sink for use out-of-process with IntelliSense support when you edit the configuration file.


### Creating a Custom In-Process Sink ###
To create a custom sink to use in-process, you must create a new sink class that implements the **IObserver&lt;EventEntry&gt;** interface. Typically, you implement the **OnNext** method from this interface as shown in the following example from the **EmailSink** class.  

```csharp
public sealed class EmailSink : IObserver&lt;EventEntry&gt;
{
  private const string DefaultSubject = "Email Sink Extension";
  private IEventTextFormatter formatter;
  private MailAddress sender;
  private MailAddressCollection recipients = new MailAddressCollection();
  private string subject;
  private string host;
  private int port;
  private NetworkCredential credentials;

  public EmailSink(string host, int port,
    string recipients, string subject, string credentials, 
    IEventTextFormatter formatter)
  {
    this.formatter = formatter ?? new EventTextFormatter();
    this.host = host;
    this.port = GuardPort(port);
    this.credentials = CredentialManager.GetCredentials(credentials);
    this.sender = new MailAddress(this.credentials.UserName);
    this.recipients.Add(GuardRecipients(recipients));
    this.subject = subject ?? DefaultSubject;
  }

  public void OnNext(EventEntry entry)
  {
    if (entry != null)
    {
      using (var writer = new StringWriter())
      {
        this.formatter.WriteEvent(entry, writer);
        Post(writer.ToString());
      }
    }
  }

  private async void Post(string body)
  {
    using (var client = new SmtpClient(this.host, this.port) 
      { Credentials = this.credentials, EnableSsl = true })
    using (var message = new MailMessage(this.sender, this.recipients[0]) 
      { Body = body, Subject = this.subject })
    {
      for (int i = 1; i &lt; this.recipients.Count; i++)
        message.CC.Add(this.recipients[i]);

      try
      {
        await client.SendMailAsync(message).ConfigureAwait(false);
      }
      catch (SmtpException e)
      {
        SemanticLoggingEventSource.Log.CustomSinkUnhandledFault(
          "SMTP error sending email: " + e.Message);
      }
      catch (InvalidOperationException e)
      {
        SemanticLoggingEventSource.Log.CustomSinkUnhandledFault(
          "Configuration error sending email: " + e.Message);
      }
    }
  }

  public void OnCompleted()
  {
  }

  public void OnError(Exception error)
  {
  }

  private static int GuardPort(int port)
  {
    if (port &lt; 0)
      throw new ArgumentOutOfRangeException("port");

    return port;
  }

  private static string GuardRecipients(string recipients)
  {
    if (recipients == null)
      throw new ArgumentNullException("recipients");

    if (string.IsNullOrWhiteSpace(recipients))
      throw new ArgumentException(
        "The recipients cannot be empty", "recipients");

    return recipients;
  }
}
```

![](images/note.gif)#!SPOKENBY(Jana)!#:&gt; 
Notice how this example uses an asynchronous method to handle sending the email message.You can use the **CustomSinkUnhandledFault** method in the **SemanticLoggingEventSource** class to log exceptions in your custom sink.  
![](images/note.gif)#!SPOKENBY(Poe)!#:&gt; 
Monitoring the events from the **SemanticLoggingEventSource** is a great way to diagnose the Semantic Logging Application Block itself. To learn how to create a custom in-process sink you should also examine the source code of some of the built-in event sinks such as the **ConsoleSink** or **SQLDatabaseSink**. The **SQLDatabaseSink** illustrates an approach to buffering events in the sink.  
Before you use the custom sink in-process, you should create an extension method to enable you to subscribe the sink to the observable event listener. The following code sample shows an extension method for the custom email sink.  

```csharp
public static class EmailSinkExtensions
{
  public static SinkSubscription&lt;EmailSink&gt; LogToEmail(
    this IObservable&lt;EventEntry&gt; eventStream, string host, int port,
    string recipients, string subject, string credentials,
    IEventTextFormatter formatter = null)
  {
    var sink = new EmailSink(host, port, recipients, subject, credentials, formatter);
 
    var subscription = eventStream.Subscribe(sink);
 
    return new SinkSubscription&lt;EmailSink&gt;(subscription, sink);
  }
}
```

Finally, to use a sink in-process, you can create and configure the event listener in code as shown in the following code sample.  

```csharp
ObservableEventListener listener = new ObservableEventListener();
listener.EnableEvents(MyCompanyEventSource.Log,
  EventLevel.LogAlways, Keywords.All);
 
listener.LogToConsole();
listener.LogToEmail("smtp.live.com", 587, "bill@adatum.com",
  "In Proc Sample", "etw");
```

You can also pass a formatter to the **LogToEmail** method if you want to customize the format of the email message.  


### Creating a Custom Out-of-Process Sink without IntelliSense Support ###
When you use the Semantic Logging Application Block out-of-process, the block creates and configures sink instances based on configuration information in an XML file. You can configure the block to use a custom sink by using the **customSink **element as shown in the following XML sample.  

```other
&lt;customSink name="SimpleCustomEmailSink"
  type ="CustomSinkExtension.EmailSink, CustomSinkExtension"&gt;
  &lt;sources&gt;
    &lt;eventSource name="MyCompany" level="Critical" /&gt;
  &lt;/sources&gt;
  &lt;parameters&gt;
    &lt;parameter name="host" type="System.String" value="smtp.live.com" /&gt;
    &lt;parameter name="port" type="System.Int32" value="587" /&gt;
    &lt;parameter name="recipients" type="System.String"
      value="bill@adatum.com" /&gt;
    &lt;parameter name="subject" type="System.String"
      value="Simple Custom Email Sink" /&gt;
    &lt;parameter name="credentials" type="System.String" value="etw" /&gt;
  &lt;/parameters&gt;
&lt;/customSink&gt;
```

In this example, the class named **EmailSink** defines the custom sink. The **parameter** elements specify the constructor arguments in the same order that they appear in the constructor.  
You can use the same custom **EmailSink** class (shown in the previous section) that implements the **IObserver&lt;EventEntry&gt;** interface in both in-process and out-of-process scenarios.  
![](images/note.gif)#!SPOKENBY(Markus)!#:&gt; 
If you are using the **customSink** element in the XML configuration file, the order of the **parameter** elements must match the order of the constructor arguments. Your custom sink class must also implement the **IObserver&lt;EventEntry&gt;** interface.

### Creating a Custom Out-of-Process Sink with IntelliSense Support ###
To make it easier to configure the custom sink, you can add support for IntelliSense when you edit the XML configuration file. You can use the same **SimpleCustomEmailSink** class shown in the previous section.  
![](images/note.gif)#!SPOKENBY(Carlos)!#:&gt; 
If your custom sink does not implement **IObserver&lt;T&gt;** where **T** is **EventEntry**, you must provide the additional configuration support shown in this section. You must also, provide a transformation from **EventEntry** to **T**: see the built-in sinks in the block for examples of how to do this.The following XML sample shows the XML that configures the custom sink and that supports IntelliSense in Visual Studio.  

```other
&lt;emailSink xmlns="urn:sample.etw.emailsink" 
  credentials="etw" host="smtp.live.com"
  name="ExtensionEmailSink" port="587"
  recipients="bill@adatum.com" subject="Extension Email Sink"&gt;
  &lt;sources&gt;
    &lt;eventSource name="MyCompany" level="Critical" /&gt;
  &lt;/sources&gt;
&lt;/emailSink&gt;
```

This example uses the custom **emailSink** element in a custom XML namespace. An XML schema defines this namespace, and enables the IntelliSense support in Visual Studio. The following XML sample shows the schema.  

```other
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;xs:schema id="EmailEventSinkElement"
    targetNamespace="urn:sample.etw.emailsink"
    xmlns="urn:sample.etw.emailsink"
    xmlns:etw=
    http://schemas.microsoft.com/practices/2013/entlib/semanticlogging/etw
    xmlns:xs="http://www.w3.org/2001/XMLSchema"           
    elementFormDefault="qualified"
    attributeFormDefault="unqualified"&gt;
 
  &lt;xs:element name="emailSink"&gt;
    &lt;xs:complexType&gt;
      &lt;xs:sequence&gt;
        &lt;xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip"/&gt;
      &lt;/xs:sequence&gt;
      &lt;xs:attribute name="name" type="xs:string" use="required" /&gt;
      &lt;xs:attribute name="host" type="xs:string" use="required" /&gt;
      &lt;xs:attribute name="port" type="xs:int" use="required" /&gt;
      &lt;xs:attribute name="credentials" type="xs:string" use="required" /&gt;
      &lt;xs:attribute name="recipients" type="xs:string" use="required" /&gt;
      &lt;xs:attribute name="subject" type="xs:string" use="optional" /&gt;
    &lt;/xs:complexType&gt;
  &lt;/xs:element&gt;
&lt;/xs:schema&gt;
```

You should place this schema file in the same folder as your XML configuration file (or in a subfolder beneath the folder that holds your XML configuration file).  
A schema file for your custom sink is optional. However, your custom sink element should be in a custom namespace to differentiate it from the built-in namespace. For example:  

```other
&lt;emailSink xmlns:noxsd="urn:no_schema" 
  ... &gt;
  &lt;sources&gt;
    ...
  &lt;/sources&gt;
&lt;/emailSink&gt;
```

In addition to the schema file, you also need a class to enable the Semantic Logging Application Block to read the custom configuration data from the XML configuration file. This **EmailSinkElement** class must implement the **ISinkElement** class and implement the **CanCreateSink** and **CreateSink** methods as shown in the following example.  

```csharp
public class EmailSinkElement : ISinkElement
{
  private readonly XName sinkName =
    XName.Get("emailSink", "urn:sample.etw.emailsink");
 
  public bool CanCreateSink(XElement element)
  {
    return element.Name == this.sinkName;
  }
 
  public IObserver&lt;EventEntry&gt; CreateSink(XElement element)
  {
    var host = (string)element.Attribute("host");
    var port = (int)element.Attribute("port");
    var recipients = (string)element.Attribute("recipients");
    var subject = (string)element.Attribute("subject");
    var credentials = (string)element.Attribute("credentials");
 
    var formatter = FormatterElementFactory.Get(element);
 
    return new EmailSink(host, port, recipients, subject,
      credentials, formatter);
  }
}
```

The **CreateSink** method creates an instance of the custom **EmailSink** type.  
Place the assembly that contains your implementations of the **ISinkElement** and **IObserver&lt;EventEntry&gt;** interfaces is the same folder as the XML configuration file.  


## Creating Custom Event Listener Host Applications ##
The Semantic Logging Application Block includes an event listener host application to use when you want to use the block out-of-process: this application can run as a console application or as a Windows Service. Internally, both of these applications use the **TraceEventService** and **TraceEventServiceConfiguration** classes.  
You can create your own custom out-of-process event listener using these same two classes. The following code sample from the out-of-process console host illustrates their use.  

```csharp
internal class TraceEventServiceHost
{
  private const string LoggingEventSourceName = "Logging";
  private const string EtwConfigurationFileName = "EtwConfigurationFileName";

  private static readonly TraceSource logSource =
    new TraceSource(LoggingEventSourceName);

  internal static void Run()
  {
    try
    {
      ...

      var configuration = TraceEventServiceConfiguration.Load(
        ConfigurationManager.AppSettings[EtwConfigurationFileName]);
      using (var service = new TraceEventService(configuration))
      {
        service.Start();
        ...
      }
    }
    catch (Exception e)
    {
      ...
    }
    finally
    {
      logSource.Close();
    }
  }

  ...
}
```

You should consider adding a mechanism that monitors the configuration file for changes. The Out-of-Process Host application included with the block uses the **FileSystemWatcher** class from the .NET Framework to monitor the configuration file for changes. When it detects a change it automatically restarts the internal **TraceEventService** instance with the new configuration settings.  


# Summary #
The Semantic Logging Application Block, while superficially similar to the Logging Application Block, offers two significant additional features.  
Semantic logging and strongly typed events ensure that the structure of your log messages is well defined and consistent. This makes it much easier to process your log messages automatically whether you want to analyze them, gain operational intelligence insights, drive some other automated process from them, or correlate them with another set of events. Creating a log message in your code now becomes a simple case of calling a method to report that an event of some significance has just happened in your application. However, you do have to write the event method, but this means you make decisions about the characteristics of the message in the correct place in your application: in your event source class.  
You can use the Semantic Logging Application Block out-of-process to capture and process log messages from another application. This makes it possible to collect diagnostic trace information from a live application while making logging more robust by handling log messages in a separate process. The block makes use of Event Tracing for Windows to achieve this: the production application can write high volumes of trace messages that ETW intercepts and delivers to your separate logging application. Your logging application can use all of the block’s sinks and formatters to process these log messages and send them to the appropriate destination.  


# More Information #
All links in this book are accessible from the book's online bibliography on MSDN at [http://aka.ms/el6biblio](http://aka.ms/el6biblio).  
For more information about ETW, see [Event Tracing](http://msdn.microsoft.com/en-us/library/windows/desktop/bb968803.aspx) on MSDN.  
For more information about the **EventSource** class and the **WriteEvent** and **IsEnabled** methods, see [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) on MSDN.  
For more information about the **EventSourceAnalyzer** class, see the topic [Checking an EventSource Class for Errors](http://go.microsoft.com/fwlink/p/?LinkID=304195)  in the Enterprise Library Reference Documentation.  
You can use the **onCompletedTimeout** parameter to the **CreateListener** method controls how long a listener will wait for the sink to flush itself before disposing the sinks. For more information, see the topic [Event Sink Properties](http://go.microsoft.com/fwlink/p/?LinkID=304196) in the Enterprise Library Reference Documentation.  
If you are collecting high volumes of trace messages from a production application, you may need to tweak the settings for the trace event service in the XML configuration file. For information about these settings, see the topic [Performance Considerations](http://go.microsoft.com/fwlink/p/?LinkID=304194) in the Enterprise Library Reference Documentation.  
**General Links:**  
+ There are resources to help if you're getting started with Enterprise Library, and there's help for existing users as well (such as the breaking changes and migration information for previous versions) available on the Enterprise Library Community Site at [http://www.codeplex.com/entlib/](http://www.codeplex.com/entlib/). 
+ For more details about the Enterprise Library Application Blocks, see the [Enterprise Library Reference Documentation](http://go.microsoft.com/fwlink/p/?LinkID=290901) and the [Enterprise Library 6 Class Library](http://msdn.microsoft.com/en-us/library/dn170426(v=pandp.60).aspx).
+ You can download and work through the Hands-On Labs for Enterprise Library, which are available at [http://aka.ms/el6hols](http://aka.ms/el6hols).
+ You can download the Enterprise Library binaries, source code, Reference Implementation, or QuickStarts from the Microsoft Download Center at [http://go.microsoft.com/fwlink/p/?LinkID=290898](http://go.microsoft.com/fwlink/p/?LinkID=290898). 
+ To help you understand how you can use Enterprise Library application blocks, we provide a series of simple example applications that you can run and examine. To obtain the example applications, go to [http://go.microsoft.com/fwlink/p/?LinkID=304210](http://go.microsoft.com/fwlink/p/?LinkID=304210). 


