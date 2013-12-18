---
Source File Name: 40-Logging.docx
AssetID: 1ee4fccf-170c-4650-8282-c46e86902a17
Title: Designing for Simple and Consistent Logging Functionality
Order In ToC: 1-6-1
Output Filename: 1-6-1_Designing for Simple and Consistent Logging Functionality.markdown
---

#### Markdown Test ####
# Designing for Simple and Consistent Logging Functionality #
----------

Developers face many implementation choices and requirements when they add logging functionality to their applications. Different applications may log information to different destinations. For example, one application may use an event log and another application may use a flat file. Even a single application may log information to multiple destinations. As a result, developers must often write duplicate code for common tasks such as writing to an event log or to a flat file.   
Uniform implementations make the code easier to understand, more predictable, and easier to maintain. However, different development teams routinely implement different logging strategies. The Logging Application Block encapsulates the logic that performs logging and tracing operations into a few classes that have small numbers of methods. These methods are the same for all log message destinations. This means that applications that use the Logging Application Block are consistent in the ways that they log information. By using the Logging Application Block, this consistency remains across single projects, multiple projects, or enterprise-scale solutions.  

# Design Implications #
The Logging Application Block was intended to simplify the task of logging information. Therefore, it had to satisfy the following design requirements:  
+ It should expose only a small number of classes and methods that a developer needs to understand.
+ It should encapsulate different types of logging information within a single class. 
+ It should make it easy to log application-specific context information.
The next sections describe how the Logging Application Block fulfills these requirements.  
<a name="desgnc_limited" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc253065091" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Limited Set of Classes and Methods #
The application block supports a small number of classes and methods that simplify the most common logging tasks. Developers can use the following classes to implement common logging scenarios:  
+ **LogWriter**. This class writes log information to configurable destinations.
+ **LogEntry**. This class encapsulates log information.
+ **Tracer**. This class writes tracing information, such as the start and stop times of activities, to configurable destinations.
You use the method **Write** on the **LogWriter** class to log information. The **Write** method has multiple overloads to support the different amounts of information an application may log. In addition, these overloads accommodate different programming styles. The simplest method accepts a string that contains the information to be logged. This is shown in the following example.  

```csharp
myLogWriter.Write("My log message");
```


```vb
myLogWriter.Write("My log message")
```

There are other overloads that accept additional information. The following example shows passes the log message, the log message's category, its priority, and the event ID.  

```csharp
myLogWriter.Write("My log message", "Debug", 2, 400);
```


```vb
myLogWriter.Write("My log message", "Debug", 2, 400)
```

<a name="designc_encapsulate" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc253065092" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Encapsulation of Common Information #
The block includes the **LogEntry** class. This class encapsulates commonly required logging information into a **LogEntry** object. You can construct a **LogEntry** object and pass it to the appropriate overload of the **LogWriter** class's **Write** method. This is shown in the following example.  

```csharp
LogEntry logEntry = new LogEntry();
logEntry.EventId = 100;
logEntry.Priority = 2;
logEntry.Message = "Informational message";
logEntry.Categories.Add("Trace");
logEntry.Categories.Add("UI Events");

myLogWriter.Write(logEntry);
```


```vb
Dim logEntry As LogEntry = New LogEntry()
logEntry.EventId = 100
logEntry.Priority = 2
logEntry.Message = "Informational message"
logEntry.Categories.Add("Trace")
logEntry.Categories.Add("UI Events")

myLogWriter.Write(logEntry)
```

The code above creates a **LogEntry** object. The object includes the event ID, the **LogEntry** object's priority, and the logging message. It belongs to the Trace and UI Events categories. The code then calls the overload of the **Write** method that accepts a **LogEntry** object.  
<a name="designc_convenient" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc253065093" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Convenient Handling of Additional Information #
The **LogEntry** class defines properties that hold commonly required types. However, developers often need to add additional context information to log entries. The same type of context information can be required for multiple log entries in the same application or in multiple applications. Because context information can be expensive to gather, certain types of information are not automatically collected. The **ExtraInformation** providers gather context information that is useful but not always necessary to collect. Examples are stack track information and COM+ information. For information about the **ExtraInformation** providers, see [Populating a Log Message with Additional Context Information](test-markdown_62843eda-e525-4531-8d26-4efddd75ccef.html).  

