---
Source File Name: 40-Logging.docx
AssetID: 47589a17-a0d7-4651-93e1-41b9bc975eb6
Title: Design of the Logging Application Block
Order In ToC: 1-6
Output Filename: 1-6_Design of the Logging Application Block.markdown
---

#### Markdown Test ####
# Design of the Logging Application Block #
----------

The Logging Application Block includes the following features:  
+ A simple and consistent way of logging event information
+ Distribution of information to multiple sources
+ Activity tracing to mark the start and end of an activity such as a use case
+ Simplified application block configuration using the configuration tools
+ Extensibility through custom trace listeners and formatters

# Design Goals #
The Logging Application Block was designed to achieve the following goals:  
+ Meet the performance requirements for the application block by ensuring that its code has minimal overhead compared to custom code that uses the **System.Diagnostics** namespace classes directly.  
+ Take advantage of the capabilities provided by the .NET Framework **System.Diagnostics** namespace classes.
+ Make message routing flexible by using a simple model for distributing log entries. 
+ Encapsulate the logic that performs the most common application logging tasks, minimizing the need for custom logging-related code. 
+ Make sure that the application block is easily and highly configurable. 
+ Make sure that the application block is extensible. 

# Design Highlights #
The following schematic illustrates the design of the Logging Application Block.   

<img src="images\648760A1729DCAEB7C3D04CDDC32933E.png" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp" />  

The following sections describe the design of individual sections of the Logging Application Block:  
+ <a href="#design_loginfo" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Log Information</a>
+ <a href="#design_facade" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Logging Facade</a>
+ <a href="#design_tracelisteners" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Trace Listeners</a>
+ <a href="#design_logfilters" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Log Filters</a>
+ <a href="#design_logformatters" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Log Formatters</a>
+ <a href="#design_wcf" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">WCF Integration</a>
<a name="_Toc253065083" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Log Information #
<a name="design_loginfo" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The **LogEntry** class encapsulates content and associated properties for the information an application sends to the application block for logging. Your client code creates new log entries and sets properties such as the message, title, priority, severity, and event ID. Other properties, such as the process name, process ID, and thread ID, are populated by the application block. A **LogEntry** object also contains the **ExtendedProperties** property. This property is a collection of name/value pairs that allows you to place arbitrary string information into the **LogEntry** object. You can also create a custom **LogEntry** object by creating a class that derives from the **LogEntry** class.   

<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>You must either create new formatters and trace listeners or modify the existing ones to support a custom **LogEntry** class that you create.</td></tr></table><p /></div>
A **LogEntry** object has a collection of categories. The application block determines the destination for the log entry by its categories. You can also prevent the application from writing log entries that specify a designated category by using category filters.   

<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>Client code can also pass information to the Logging Application Block without constructing a **LogEntry** object by using overloads of the **LogWriter.Write** method that do not require a **LogEntry** object. The application block will construct a **LogEntry** object for its internal use. </td></tr></table><p /></div><a name="_Toc253065084" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Logging Facade #
<a name="design_facade" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The **LogWriter** class is a façade that sends a log entry to one or more destinations. The client code sends the information to the **LogWriter** instance, which then checks the filters to see if the **LogEntry** object should be discarded or sent to any of the trace sources. The filters are implementations of the **ILogFilter** interface. You specify the filter implementations in the configuration information. If the **LogEntry** object passes through the filters, the **LogWriter** instance sends it to each trace source identified by a name that matches one of the **LogEntry** object's categories.  

<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>Whenever a **Log Entry** object fails within the **LogWriter** instance, the **LogWriter** instance writes a **LogEntry** object to the **ErrorSource** special trace source. The **EventID** is always the **LogWriterFailureEventID**. The **EventID** number is 6352.</td></tr></table><p /></div>
The application block relies on the **LogSource** class to route **LogEntry** objects. The **LogWriter** instance has a collection of **LogSource** objects that correspond to the sources defined in the configuration file. The application block uses the collection of categories in a **LogEntry** object to determine the **LogSource** object to use for writing the **LogEntry**. Categories map directly to **LogSource** object names.   
The **LogWriter** object also has three special **LogSource** objects that receive any **LogEntry** object that contains a category that does not have an associated **LogSource** object, information about errors or warnings that occur while the Logging Application Block code is executing, and all **LogEntry** objects. It does not process errors or warnings from the application code.  
<a name="_Toc253065085" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Trace Listeners #
<a name="design_tracelisteners" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Each trace source contains a (potentially empty) collection of trace listeners. A trace listener is a class that derives from the abstract class **TraceListener** in the **System.Diagnostics** namespace. Examples of trace listeners are the Windows Event Log, a text file, and Message Queuing. Although any **TraceListener** instance will work with the Logging Application Block, only those trace listeners that are provided with the Logging Application Block can format a **LogEntry** object. The trace listeners that the .NET Framework provides can be used by the Logging Application Block, but they only send strings.  
The Logging Application Block trace listeners do not require a **LogEntry** object or a formatter to be instantiated. Instead, you can construct them by including the necessary information in the configuration file. The **FormattedEventLogTraceListener**, and the **FlatFileTraceListener** trace listeners use the same configuration information as the **System.Diagnostics** trace listeners. This means you can configure your application to use these three trace listeners in the &lt;**system.diagnostics**&gt; configuration section of your configuration file.   
The **MsmqTraceListener**, **FormattedDatabaseTraceListener**, and **EmailTraceListener **trace listeners require more configuration information. For example, the **EmailTraceListener** trace listener requires the destination e-mail address, the SMTP server name, and related e-mail information. In addition you can take advantage of the **EmailTraceListener** trace listener **authenticationMode** and **useSSL** properties to make e-mail messages exchanged with SMTP servers less susceptible to tampering while in transit. You can configure your application to use these trace listeners by including the appropriate information in the configuration file. However, because these trace listeners require more information than the **System.Diagnostics** trace listeners, you cannot configure them in the &lt;**system.diagnostics**&gt; configuration section. Instead, you should use the &lt;**listeners**&gt; section located in the Logging Application Block configuration section. You can also construct these trace listeners in code and pass the required configuration information as parameters to their constructors.   
The application block populates the collection of trace listeners from the application configuration data. The **LogSource** object sends the **LogEntry** object to each trace listener in the collection.   
<a name="_Toc253065086" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Log Filters #
<a name="design_logfilters" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>You can define configuration settings that control filtering of **LogEntry** objects. Filtering a **LogEntry** object prevents the application block from sending it to any trace source. The application block includes three types of filters. The **CategoryFilter** filters **LogEntry** objects by their category collection. The **PriorityFilter** filters **LogEntry** objects by their priority property. The **LogEnabledFilter** is a global filter that applies to all **LogEntry** objects in an application. With it, you can turn on and turn off logging. You can also create your own log filters. Use the configuration tools to add them to the application block's configuration information.  
<a name="_Toc253065087" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Log Formatters #
<a name="design_logformatters" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Log entry information must often be formatted before it is written to the destination. Trace listeners that derive from the **FormattedTraceListenerBase** class format the log entry information before writing it to a destination. The application block includes two classes for formatting **LogEntry** information. (You can also build your own formatters.) The **TextFormatter** class converts the **LogEntry** into a text string. The contents of the string are determined by replacing tokens in the **Template** property of the **TextFormatter** configuration information in your application configuration file. The **BinaryLogFormatter** class uses the .NET Framework **BinaryFormatter** to serialize and deserialize the LogEntry in binary format. It should be used with the Message Queuing distributor service.   
<a name="WCF_Integration" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc253065088" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# WCF Integration #
<a name="design_wcf" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The Logging Application Block includes three classes that, together, allow you to integrate it with applications that use WCF. These classes are the following:  
+ XmlLogEntry
+ EntLibLoggingProxyTraceListener
+ XmlTraceListener
The **XmlLogEntry** class derives from the **LogEntry** class, but it includes an **Xml** property that allows it to contain the original XML data provided by WCF as well as the information that is contained in the **LogEntry** class.  
The **EntLibLoggingProxyTraceListener **class is used in WCF’s **System.ServiceModel** trace source so that it can receive messages from WCF. It adds this information to an **XmlLogEntry** object and then sends the object to the Logging Application Block, which processes it.   
The **XmlTraceListener** class derives from the .NET **XmlWriterTraceListener** class. It extracts XML data from an **XmlLogEntry** object and writes this data to an XML text file. You can analyze the output of this trace listener with the WCF log file analysis tools.  

