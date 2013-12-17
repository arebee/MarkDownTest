---
Source File Name: 40-Logging.docx
AssetID: 3712145d-7fa5-4fd7-b9a7-ea2d018b5fc7
Title: Populating and Raising Events from Code
Order In ToC: 1-5-7
Output Filename: 1-5-7_Populating and Raising Events from Code.markdown
---

#### Markdown Test ####
# Populating and Raising Events from Code #
----------

The ability to populate and raise events from code is fundamental to the functionality of the Logging Application Block.   

# Typical Goals #
In this scenario, you want to specify the data to be logged, along with a category and priority, so that the application block can log the events to the appropriate trace listener or listeners.  

# Solution #
Configure the application to use the Logging Application Block. Use the Enterprise Library configuration tools to configure categories and trace listeners to be used. Create the information that is submitted to the Logging Application Block in the **LogEntry** object. Call the **Write** method on the **LogWriter** class, passing the **LogEntry** object.  

![](images/note.gif)Note:The **LogWriter** class includes overloads that allow you to pass event information without constructing a **LogEntry** object. This scenario only demonstrates one overload provided by the **LogWriter** class. For information about the other overloads, see the Logging Application Block API reference documentation.

The default class for writing a **LogEntry** in earlier versions of Enterprise Library was the static **Logger** class. Code that uses the **Logger** class will continue to work in this release. For information about resolving Enterprise Library objects in your applications, see <a href="test-markdown_bfd186b8-9a32-477a-bee7-14742ba1ca42.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Creating and Referencing Enterprise Library Objects</a>. For information about the **Logger** class, see the online documentation for Enterprise Library 4.1, available on the <a href="http://msdn.microsoft.com/en-gb/library/dd203099.aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">MSDN Web Site</a>.<a name="_Toc253065056" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Creating and Writing a LogEntry #
The following code shows how to create a **LogEntry** object and use the **Write **method of the **LogWriter** class**.** The **LogEntry** object has a priority of 2 and belongs to both the **Trace** and **UI Events** categories.  It assumes you have created an instance of the **LogWriter** class and saved it in a variable named **myLogWriter**.  

```C#
LogEntry logEntry = new LogEntry();

logEntry.EventId = 100;
logEntry.Priority = 2;
logEntry.Message = "Informational message";
logEntry.Categories.Add("Trace");
logEntry.Categories.Add("UI Events");

myLogWriter.Write(logEntry);
```


```Visual Basic
Dim logEntry As New LogEntry()

logEntry.EventId = 100
logEntry.Priority = 2
logEntry.Message = "Informational message"
logEntry.Categories.Add("Trace")
logEntry.Categories.Add("UI Events")

myLogWriter.Write(logEntry)
```

For information about how to create a **LogWriter** instance, see <a href="test-markdown_875469ce-1185-4690-9d1c-36d452bf6a4a.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Creating LogWriter and TraceManager Instances</a>.  
<a name="_Toc253065057" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Usage Notes #
If the message cannot be written to the configured destination, the information is written to the listeners configured for the **Logging Errors &amp; Warnings** special category.  
If you have configured the **All Events** special source, the event information will be written to that source as well as the sources associated with the categories specified in the **LogEntry** object.  
For applications configured to write to an event log trace listener that specifies a custom event log, and the custom event log does not exist at the time your component writes a log entry, the Logging Application Block will attempt to create that event log. To create the event log successfully, the component using the Logging Application Block must be running with the appropriate privileges. Specifically, it must have full access rights to the registry key **HKLM\System\CurrentControlSet\Services\EventLog**. If applications using the Logging Application Block will not have the appropriate access rights, custom event logs should be created by running installation programs under accounts with the necessary privileges.   

