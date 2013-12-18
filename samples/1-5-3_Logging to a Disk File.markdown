---
Source File Name: 40-Logging.docx
AssetID: d0234cae-d49b-44b0-9f0c-bb79089022af
Title: Logging to a Disk File
Order In ToC: 1-5-3
Output Filename: 1-5-3_Logging to a Disk File.markdown
---

#### Markdown Test ####
# Logging to a Disk File #
----------

This scenario is one of several that describe the typical requirements when using the Logging Application Block in your applications. It describes the process for setting up the block to send log events to a disk file. The process involves configuring the block and performing other tasks to prepare your application. This topic acts as a quick reference to help you quickly set up the Logging Application Block to perform the required logging action.  

# Typical Goals #
You need to send logging information created as log entries to one or more sinks or targets, and the list of targets includes a disk file. You want to set up the Logging Application Block to use a suitable trace listener, decide what type of file to create (text or XML), and configure the format and other file logging options you require.   

# Solution #
The high-level stages of the process that you should follow are to:  
+ Decide on the type of disk file you want to use, how you want to roll over and create new files, and the required log entry format.
+ Configure your application to use the Logging Application Block with a flat file trace listener, a rolling flat file trace listener, or an XML trace listener.
+ Configure the properties of the trace listener you selected.
+ Configure the required filters and log sources for the Logging Application Block.
+ Add references to the Logging Application Block to your application and write code to send log entries to the block. 
The following steps describe the whole process in more detail.  
**To configure logging to a disk file**

1. Decide on the format for the disk file. You can output the information in log entries as XML or as simple text. If you want an XML format you must choose the XML trace listener. If you want a text format for the output, you can choose either the flat file trace listener or the rolling flat file trace listener. The major difference between these two types is that the rolling flat file trace listener allows you to start a new log file at specified intervals.  
2. Add the Logging Application Block to your application configuration, and then add the type of trace listener you chose in the previous step to the block configuration, as shown in the topic [Configuring Trace Listeners](test-markdown_a0ea0d8b-7675-48b8-9b5f-9d6d8e2382f0.html). 
3. Configure the properties of your chosen trace listener. Enter the filename for the file the trace listener will create as the **File Name** property. You can include environment variables such as %WINDIR%, %TEMP%, and %USERPROFILE% in the name if required. If you chose the rolling flat file trace listener, the block will add the current date and an incrementing number to the filename of archived log files, such as **mylog2007-01-10.1.log**, and will use the **File Name** property for the current log file. However, you can change the format it uses for the date and time by editing the value of the **Timestamp Pattern** property of the rolling flat file trace listener. For more information about configuring trace listeners, see the tables of properties and descriptions in the topic [Trace Listener Properties](test-markdown_b45ee518-82b1-426c-b772-1e6c0fde455e.html).
4. If you chose the flat file trace listener or the rolling flat file trace listener, edit the values of the **Message Header **and **Message Footer **properties if required. These specify the text to output at the start and end of the file.  
5. If you chose the flat file trace listener or the rolling flat file trace listener, select the text formatter for the **Formatter Name** property, or add a new text formatter to the configuration and select this. If required, edit the **Template** property of the text formatter to specify the actual text format you want. You can change the text of the log message, and insert placeholders for the values exposed by the log entry. For more details, see [Configuring Formatters](test-markdown_8b4b7563-0062-4690-bfc2-df37f15b2d35.html).
6. If you chose the rolling flat file trace listener, set the values for the properties that control when it will start a new log file. Set the **File Exists Behavior** property to either **Overwrite** to replace existing archive files if one exists for the same date and time, or **Increment** if you want to create a new file with an incremented file name such as **mylog2007-01-10.2.log**. Set the **Roll Interval** property to specify when the block should start a new log file. The options are **None** (the default), **Midnight**, and intervals of **Minute**, **Hour**, **Day**, **Week**, **Month**, or **Year**. You can also force the block to start a new file when the existing one reaches a predetermined size by setting the **Roll Size KB** property to the maximum file size in KB. 
7. If you chose the rolling flat file trace listener, decide if you want to specify a limit to the number of retained archive log files. You can set the **Max Archived Files** property to a numerical value, and the block will delete old log files based on the creation date where the number exceeds this value. 
8. Set the **Severity Filter** property of the trace listener you chose to specify the level of messages that will be logged. The valid values are **All** (the default), **Off**, **Critical**, **Error**, **Warning**, **Information**, **Verbose**, and **ActivityTracing**. The setting effectively means "the specified level and everything more important." For example, the **Warning** setting will detect warnings, errors, and critical events.
9. Configure any trace sources you require. In general, you will add additional categories if you want to be able to assign log entries to more than the default **General** category. You may also want to change the settings for the **Logging Errors &amp; Warnings**, **Unprocessed Category**, and **All Events** special trace sources. For example, you may want to send entries for logging errors to the Windows Event Log or a disk file. For more information on configuring trace sources, see the topic [Configuring Trace Source Categories](test-markdown_9301547d-44c4-490c-91a0-b63e86e4b6a2.html). 
10. Configure any log filters you require. You may want to filter log entries arriving at the Logging Application Block by category or by priority, or completely enable or disable logging. For more information on configuring log filters, see the topic [Configuring Logging Filters](test-markdown_ac913544-cc72-4de9-b916-f9d85d473685.html).
11. Add references to the Logging Application Block and other required assemblies to your application. For more details, see [Adding Application Code](test-markdown_730d69d7-7e0f-4b21-8ab8-725bcec1bfd3.html).
12. Write code to create log entries and send them to the Logging Application Block. For more information, see the topic [Populating and Raising Events from Code](test-markdown_3712145d-7fa5-4fd7-b9a7-ea2d018b5fc7.html) and [Populating a Log Message with Additional Context Information](test-markdown_62843eda-e525-4531-8d26-4efddd75ccef.html).

