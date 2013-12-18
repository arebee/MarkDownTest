---
Source File Name: 40-Logging.docx
AssetID: bca61231-bddd-4531-b797-449dfc0639e8
Title: Extending the Logging Application Block
Order In ToC: 1-7-1
Output Filename: 1-7-1_Extending the Logging Application Block.markdown
---

#### Markdown Test ####
# Extending the Logging Application Block #
----------

The Logging Application Block is designed to suit a variety of applications and to provide the most commonly used logging functions. You can extend the application block through designated extension points. Typically, these are custom classes, written by you, that implement a particular interface or derive from an abstract class. Because these custom classes exist in your application space, you do not need to modify or rebuild the application block. Instead, you designate your extensions using configuration settings. Additionally, with extension points, you can adapt the application block to suit the needs of any particular application.   
You can extend the capabilities of the block by adding custom formatters, trace listeners, and log filters.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Custom Provider or Extension</p></th><th><p>Interface or Base Class</p></th></tr><tr><td><p>Log Entry Formatter</p></td><td><p> ILogFormatter</p></td></tr><tr><td><p>Trace Listener</p></td><td><p>CustomTraceListener</p></td></tr><tr><td><p>Log Filter</p></td><td><p>ILogFilter</p><p>LogFilter</p></td></tr></table>
For detailed information about how to integrate custom providers with the Enterprise Library configuration system and configuration tools see [Creating Custom Providers for Enterprise Library](test-markdown_3d7d908a-3382-4d75-9909-c968dfade305.html).  
The following procedure describes the general approach to extend the Logging Application Block.  
**To extend the Logging Application Block**

1. Create a new custom class and add it to your project. 
2. Make sure that the class implements the required interfaces, constructors, and methods. 
3. Add the custom object to the configuration of the Logging Application Block using the Enterprise Library configuration tools: + Specify your custom class as the type name. 
+ Specify any custom configuration properties by modifying the attributes of the object. 


# Creating a Custom Log Entry Formatter #
You can create a new log entry formatter by implementing the interface **ILogFormatter**. Your custom class must carry the attribute **ConfigurationElementType** with the type **CustomFormatterData** as the attribute parameter.  

```csharp
[ConfigurationElementType(typeof(CustomFormatterData))]
public class MyFormatter : ILogFormatter
```


```vb
&lt;ConfigurationElementType(GetType(CustomFormatterData))&gt; _
Public Class MyFormatter
  Implements ILogFormatter
```

Your class must implement the **Format** method that accepts a **LogEntry** instance and returns the formatted data as a string.   

# Creating a Custom Trace Listener #
In some cases, the trace listeners provided with the Logging Application Block will not satisfy your application's requirements, and you will need to create your own trace listeners.  
You can create a new trace listener by extending the **CustomTraceListener** class. Your custom class must carry the attribute **ConfigurationElementType** with the type **CustomFormatterData** as the attribute parameter.  

```csharp
[ConfigurationElementType(typeof(CustomTraceListenerData))]
public class MyTraceListener : CustomTraceListener
```


```vb
&lt;ConfigurationElementType(GetType(CustomTraceListenerData))&gt; _
Public Class MyTraceListener
  Inherits CustomTraceListener
```

Your class must override the **TraceData** method to** **format the **LogEntry** object and write the information to the output destination.    
The Logging Application Block trace listeners derive from the **TraceListener** class in the **System.Diagnostic** namespace. This means that applications that do not use the application block can still use the application block trace listeners. To allow your custom trace listener to execute correctly when it is used outside the Logging Application Block, you should do the following:  
+ In the **TraceData** method, test to see if the data parameter is of type **LogEntry**. If it is not of type **LogEntry**, use the **ToString** method to format the object.
+ Override the **Write** and **WriteLine** methods.

# Creating a Custom Log Filter #
You can implement a custom Log Filter by implementing the **ILogFilter** interface or by extending the **LogFilter** base class. However, you must be aware of an issue that can prevent application code from resolving the configured name of the provider - although this is only an issue where you wish to query the collection of filters when checking if a log entry will be specifically blocked by this filter.   
The **ILogFilter** interface defines a **Name** property that should return the name of the instance of the custom log filter from the configuration. However, there currently is no way to retrieve that name from within your custom log filter. Instead, you can pass a key\value pair in the **NameValueCollection** received by the constructor, and use this to set the **Name** property of the filter. When configuring your custom log filter, you will have to duplicate the name: once for the actual name of that instance of the custom log filter, and again in the named property collection that is passed to the constructor.  

![](images/note.gif)Note:&gt; If you intend to modify or extend the **LogEntry** class, you may want to use the **ReflectedPropertyToken** class. This formatting token allows you to log custom properties on custom classes that either derive from the **LogEntry** class or modify it.** **This token has the syntax {property(<i>CustomPropertyName</i>)}, and it uses reflection to obtain the value of the specified property and log it** **to the chosen trace listener. For example, you might have a property named **FileName** that is the name of the log file. Using this token allows you to insert this name into a log entry.
