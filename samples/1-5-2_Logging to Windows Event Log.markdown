---
Source File Name: 40-Logging.docx
AssetID: 45e42198-6a44-4d0b-bb55-691b7c5ed2bf
Title: Logging to Windows Event Log
Order In ToC: 1-5-2
Output Filename: 1-5-2_Logging to Windows Event Log.markdown
---

#### Markdown Test ####
# Logging to Windows Event Log #
----------

This scenario is one of several that describe the typical requirements when using the Logging Application Block in your applications. It describes the process for setting up the block to send log events to Windows Event Log. The process involves configuring the block and performing other tasks to prepare your application. This topic acts as a quick reference to help you rapidly set up the Logging Application Block to perform the required logging action.  

# Typical Goals #
You need to send logging information created as log entries to one or more sinks or targets, and the list of targets includes Windows Event Log. You want to set up the Logging Application Block to use a suitable trace listener, format the log entries, and create the target event log if you do not want to use one of the existing event logs (generally Application, System, or Security with additional logs available in Windows Vista® and Windows Server® 2008).   

# Solution #
The high-level stages of the process that you should follow are to:  
+ Configure any additional event logs you require in Windows Event Log.
+ Configure your application to use the Logging Application Block with an event log trace listener.
+ Configure the properties of the event log trace listener.
+ Configure the required filters and log sources for the Logging Application Block.
+ Add references to the Logging Application Block to your application and write code to send log entries to the block. 
The following steps describe the whole process in more detail.  
**To configure logging to Windows Event Log**

1. Decide if you need to create a new event log, or if you will publish events into one of the existing event logs such as Application, System, Security, or one of the many other event logs available in Windows Vista and Windows Server 2008 and later. If you decide to create a new event log, you can do so using the Event Log Viewer interface or by writing code. For more information, see [EventLogInstaller Class]({$xref}) on MSDN. 
2. Add the Logging Application Block to your application configuration, and then add a formatted event log trace listener to the block configuration as shown in the topic [Configuring Trace Listeners]({$finalDocSet}). 
3. Configure the properties of the event log trace listener. Enter the name of the event log you want to write entries to (usually this is the Application log) for the **Log Name** property, the name of the computer if you want to write to Windows Event Log on a remote machine for the **Machine Name** property, and the text you want to appear as the source of the events for the **Source Name** property. If you are logging to a remote machine, you must configure appropriate account permissions on the target computer. For more information about configuring the event log trace listener, see the table of properties and descriptions in the topic [Trace Listener Properties]({$finalDocSet}).  
4. Select the text formatter for the **Formatter Name** property of the event log trace listener, or add a new text formatter to the configuration and select this. If required, edit the **Template** property of the text formatter to specify the actual text format you want. You can change the text of the log message, and insert placeholders for the values exposed by the log entry. For more details, see [Configuring Formatters]({$finalDocSet}).
5. Set the **Severity Filter **property of the event log trace listener to specify the level of messages that will be logged. The valid values are **All** (the default), **Off**, **Critical**, **Error**, **Warning**, **Information**, **Verbose**, and **ActivityTracing**. The setting effectively means "the specified level and everything more important." For example, the **Warning** setting will detect warnings, errors, and critical events.
6. Configure any trace sources you require. In general, you will add additional categories if you want to be able to assign log entries to more than the default "General" category. You may also want to change the settings for the **Logging Errors &amp; Warnings**, **Unprocessed Category**, and **All Events** special trace sources. For example, you may want to send entries for logging errors to the Windows Event Log or a disk file. For more information on configuring trace sources, see the topic [Configuring Trace Source Categories]({$finalDocSet}). 
7. Configure any log filters you require. You may want to filter log entries arriving at the Logging Application Block by category or by priority, or completely enable or disable logging. For more information on configuring log filters, see the topic [Configuring Logging Filters]({$finalDocSet}).
8. Add references to the Logging Application Block and other required assemblies to your application. For more details, see [Adding Application Code]({$finalDocSet}).
9. Write code to create log entries and send them to the Logging Application Block. For more information, see the topic [Populating and Raising Events from Code]({$finalDocSet}) and [Populating a Log Message with Additional Context Information]({$finalDocSet}).

