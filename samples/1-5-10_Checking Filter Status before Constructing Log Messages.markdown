---
Source File Name: 40-Logging.docx
AssetID: 37d65dcf-7447-46f6-97ec-6208b9a317b6
Title: Checking Filter Status before Constructing Log Messages
Order In ToC: 1-5-10
Output Filename: 1-5-10_Checking Filter Status before Constructing Log Messages.markdown
---

#### Markdown Test ####
# Checking Filter Status before Constructing Log Messages #
----------

By using the Logging Application Block, you can query the filter status to determine whether a log message should be logged according to the filter configuration.   

# Typical Goals #
In this scenario, you want to avoid collecting context information for a log message if the current application block configuration indicates that the information will not be logged.  

# Solution #
Create a **LogEntry** object that contains the information that will be submitted to the Logging Application Block. Call the **ShouldLog** method of the **LogWriter** class, passing to it the **LogEntry** object.   
The following code shows how to create a **LogEntry** object and use the **ShouldLog **method. It assumes you have created an instance of the **LogWriter** class and saved it in a variable named **myLogWriter**.  

```csharp
LogEntry logEntry = new LogEntry();
logEntry.Priority = 2;
logEntry.Categories.Add("Trace");
logEntry.Categories.Add("UI Events");

if (myLogWriter.ShouldLog(logEntry))
{
  // Event will be logged according to current configuration. Perform operations (possibly
  // expensive) to gather additional information for the event to be logged. 
}
else
{
  // Event will not be logged. Your application can avoid the performance
  // penalty of collecting information for an event that will not be logged.
}

```


```visualbasic
Dim logEntry As New LogEntry()
logEntry.Priority = 2
logEntry.Categories.Add("Trace")
logEntry.Categories.Add("UI Events")

If myLogWriter.ShouldLog(logEntry) Then
  ' Event will be logged according to current configuration. Perform operations (possibly 
  ' expensive) to gather additional information for the event to be logged. 
Else
  ' Event will not be logged. Your application can avoid the performance
  ' penalty of collecting information for an event that will not be logged.
End If
```

For information about how to create a **LogWriter** instance, see <a href="test-markdown_875469ce-1185-4690-9d1c-36d452bf6a4a.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Creating LogWriter and TraceManager Instances</a>.  


# Usage Notes #
You can query a specific filter type to determine whether a log message should be logged according to the configuration settings for that filter type.  
The following code shows how to query the **CategoryFilter.**   

```csharp
ICollection&lt;string&gt; categories = new List&lt;string&gt;(0);
categories.Add("Trace");
categories.Add("UI Events");

if (myLogWriter.GetFilter&lt;CategoryFilter&gt;().ShouldLog(categories))
{
  // Events with these categories should be logged. 
}
```


```visualbasic
Dim categories As ICollection(Of String) = New List(Of String)(0)
categories.Add("Trace")
categories.Add("UI Events")

If myLogWriter.GetFilter(Of CategoryFilter)().ShouldLog(categories) Then
  ' Events with these categories should be logged.
End If
```


