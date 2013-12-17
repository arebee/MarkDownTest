---
Source File Name: 40-Logging.docx
AssetID: 81fb2f5e-419c-4189-a821-76982644eb8f
Title: Configuring Trace Source Categories
Order In ToC: 1-4-1-1-4
Output Filename: 1-4-1-1-4_Configuring Trace Source Categories.markdown
---

#### Markdown Test ####
# Configuring Trace Source Categories #
----------

When you write a log message from your application code, you specify a category for the log message. The category determines which listener or listeners receive the log message. There are two different category types:  
+ **Categories** that specify the logging categories you want to use. When you create a log entry, you can assign it to one or more categories. Each category can filter the log entry based on its severity (such as **Critical**, **Error**, **Warning**, or **Information**), and route it to one or more trace listeners (logging targets).
+ **Special Categories **that automatically receive all log entries, unprocessed log entries, or log entries where an error occurred during logging.  Each of these special categories can filter the log entry based on its severity (such as **Critical**, **Error**, **Warning**, or **Information**), and route it to one or more trace listeners (logging targets).

# Configuring Trace Source Categories #
To configure a trace source category programmatically, you first need to instantiate a **LoggingConfiguration** object. Then invoke the **AddLogSource** method to define a trace source category. The following code sample shows two trace source categories being configured, one named “Database” with a single trace listener, and one named “DiskFiles” with two trace listeners.  

```C#
LoggingConfiguration config = new LoggingConfiguration();

config.AddLogSource("Database", SourceLevels.All,
                     true).AddTraceListener(databaseTraceListener);
config.AddLogSource("DiskFiles", SourceLevels.All,
                     true).AddTraceListener(flatFileTraceListener);
config.LogSources["DiskFiles"].AddTraceListener(xmlTraceListener);
```

The **level** parameter specifies a severity filter for the trace source category and is a value from the **SourceLevels** enumeration: the possible values are **All** (the default), **Off**, **Critical**, **Error**, **Warning**, **Information**, **Verbose**, and **ActivityTracing**. The setting effectively means "the specified level and everything more important."  
The **autoFlush** parameter is a Boolean value that specifies whether the listener will flush its data to the target after every new entry is written to this source. When set to **False**, code can flush the entries when required.   
![](images/note.gif)Note:Note that if **autoFlush** is set to **False**, it is your responsibility to ensure that all entries are flushed to the target by invoking the listener’s **Flush** method, especially if an exception or failure occurs in the application. Otherwise, you will lose any cached logging information not yet written to the target.
# Configuring Special Trace Source Categories #
To configure the special trace source categories, you use the **SpecialSources.Unprocessed**, **SpecialSources.LoggingErrorsAndWarnings**, and **SpecialSources.AllEvents** properties of the **LoggingConfiguration** class. Each of these has an **AddListener** method for the listeners you want to use, a **Level** property to specify a severity filter, and an **AutoFlush** property to control flushing behavior. The following code sample shows how to add an event listener to the **Logging Errors &amp; Warnings** special category:  

```C#
LoggingConfiguration config = new LoggingConfiguration();

config.SpecialSources.LoggingErrorsAndWarnings.AddListener
  (eventLogTraceListener);
```

The **Level** property specifies a severity filter for the trace source category and is a value from the **SourceLevels** enumeration: the possible values are **All** (the default), **Off**, **Critical**, **Error**, **Warning**, **Information**, **Verbose**, and **ActivityTracing**. The setting effectively means "the specified level and everything more important."  
The **AutoFlush** property is a Boolean value that specifies whether the listener will flush its data to the target after every new entry is written to this source. When set to **False**, code can flush the entries when required.   
![](images/note.gif)Note:Note that if **AutoFlush** is set to **False**, it is your responsibility to ensure that all entries are flushed to the target by invoking the listener’s **Flush** method, especially if an exception or failure occurs in the application. Otherwise, you will lose any cached logging information not yet written to the target.
