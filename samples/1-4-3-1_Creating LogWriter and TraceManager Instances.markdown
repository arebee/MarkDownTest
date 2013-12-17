---
Source File Name: 40-Logging.docx
AssetID: 875469ce-1185-4690-9d1c-36d452bf6a4a
Title: Creating LogWriter and TraceManager Instances
Order In ToC: 1-4-3-1
Output Filename: 1-4-3-1_Creating LogWriter and TraceManager Instances.markdown
---

#### Markdown Test ####
# Creating LogWriter and TraceManager Instances #
----------

Before you can use the features of the Logging Application Block in your application you must create an instance of the **LogWriter** class in your code and optionally an instance of the **TraceManager** class. The **LogWriter** class contains the overloaded implementations of the **Write** method that you will use to write log messages.  
When you create an instance of the **LogWriter** class you must populate it with details of your logging configuration; this configuration is typically defined in your application configuration file. The following code sample shows how to load the configuration settings from the configuration file and create a **LogWriter** instance.  

```csharp
LogWriterFactory logWriterFactory = new LogWriterFactory();
LogWriter defaultWriter = logWriterFactory.Create();
```


```visualbasic
Dim logWriterFactory As New LogWriterFactory()
Dim defaultWriter As LogWriter = logWriterFactory.Create()
```

You can use the **LogWriter** instance to configure a **TraceManager** instance. The **TraceManager** class includes a constructor that takes a **LogWriter** instance as a parameter.   
For more information about creating Enterprise Library application block instances, see <a href="test-markdown_6453869a-c2bb-4494-9095-ea97d328ee94.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Creating Application Block Objects</a>.  

