---
Source File Name: 40-Logging.docx
AssetID: 7dd3bd7e-c463-4dbd-92f8-472e0eab8d53
Title: Logging to Windows Message Queuing
Order In ToC: 1-5-4
Output Filename: 1-5-4_Logging to Windows Message Queuing.markdown
---

#### Markdown Test ####
# Logging to Windows Message Queuing #
----------

This scenario is one of several that describe the typical requirements when using the Logging Application Block in your applications. It describes the process for setting up the block to send log events as messages through Windows Message Queuing. The process involves configuring the block and performing other tasks to prepare your application. This topic acts as a quick reference to help you quickly set up the Logging Application Block to perform the required logging action.  

# Typical Goals #
You need to send logging information created as log entries to one or more sinks or targets, and the list of targets includes Windows Message Queuing. You want to set up the Logging Application Block to use a suitable trace listener, install and configure the Distributor Service (explained below), and specify the queue and message options you require.   

# Solution #
The high-level stages of the process that you should follow are to:  
+ Identify the names of the Message Queuing queues you want to use, and the related options and requirements for the Message Queuing instance you want to use.
+ Configure your application to use the Logging Application Block with a message queuing trace listener.
+ Configure the properties of the message queuing trace listener.
+ Configure the required filters and log sources for the Logging Application Block.
+ Add references to the Logging Application Block to your application and write code to send log entries to the block. 
The following steps describe the whole process in more detail.  
**To configure logging to Windows Message Queuing**

1. The message queuing trace listener uses a separate Windows service, the Distributor Service, to send log entries to Windows Message Queuing. You must install and configure this service before you can use the message queuing trace listener. Part of the configuration involves using the message queuing tools to create a queue to which you will send log entries. You specify this name, and other details of your message queuing configuration, in the properties of the message queuing trace listener as shown later in this procedure. For information about installing, configuring, and starting the distributor service, see [Using the Distributor Service](test-markdown_ef65d516-04b0-44c2-a750-4e15aab636fd.html).  
2. Add the Logging Application Block to your application configuration, and then add a message queuing trace listener to the block configuration as shown in the topic [Configuring Trace Listeners](test-markdown_a0ea0d8b-7675-48b8-9b5f-9d6d8e2382f0.html). 
3. Configure the properties of the message queuing trace listener. Many of these properties define how the message queuing trace listener will interact with Windows Message Queuing. You must specify the path and name of the queue as the **Queue Path** property. You can also specify the priority for the message while it is in Message Queuing as the **Message Priority** property (the default in **Normal**), if delivery must be guaranteed as the **Recoverable** property (the default is **False**), whether to authenticate and encrypt the message as the **Use Authentication** and **Use Encryption** properties (the defaults for both are **False**), and the type of transaction Message Queuing should use (the default is **None**). You can also change the delivery time limits by editing the **Time To Be Received** and **Time To Reach Queue** properties, and specify if undelivered messages should be sent to a dead letter queue as the **Use Dead Letter Queue** property (the default is **False**). For more information about configuring the message queuing trace listener, see the table of properties and descriptions in the topic [Trace Listener Properties](test-markdown_b45ee518-82b1-426c-b772-1e6c0fde455e.html).  
4. Add a new binary log message formatter to your configuration, and select this for the **Formatter Name** property of the message queuing trace listener. The only property you can change for the binary formatter is the name. You <i>cannot</i> use a text formatter with the message queuing trace listener. For more details, see [Configuring Formatters](test-markdown_8b4b7563-0062-4690-bfc2-df37f15b2d35.html).
5. Set the **Severity Filter** property of the message queuing trace listener to specify the level of messages that will be logged. The valid values are **All** (the default), **Off**, **Critical**, **Error**, **Warning**, **Information**, **Verbose**, and **ActivityTracing**. The setting effectively means "the specified level and everything more important." For example, the **Warning** setting will detect warnings, errors, and critical events.
6. Specify the required settings for the **Trace Output Options** property of the message queuing trace listener. This allows you to include a range of information in the log entry, such as the date and time, the call stack, and the process ID. For more information about setting the **Trace Output Options** property of the message queuing trace listener, see the table of possible values and descriptions in the topic [TraceOutputOptions Values](test-markdown_e0902ea8-47a8-465f-a7d4-6809f35f6bc8.html).
7. Configure any Trace Sources you require. In general, you will add additional categories if you want to be able to assign log entries to more than the default **General** category. You may also want to change the settings for the **Logging Errors &amp; Warnings**, **Unprocessed Category**, and **All Events** special trace sources. For example, you may want to send entries for logging errors to the Windows Event Log or a disk file. For more information on configuring trace sources, see the topic [Configuring Trace Source Categories](test-markdown_9301547d-44c4-490c-91a0-b63e86e4b6a2.html). 
8. Configure any log filters you require. You may want to filter log entries arriving at the Logging Application Block by category or by priority, or completely enable or disable logging. For more information on configuring log filters, see the topic [Configuring Logging Filters](test-markdown_ac913544-cc72-4de9-b916-f9d85d473685.html).
9. Add references to the Logging Application Block and other required assemblies to your application. For more details, see [Adding Application Code](test-markdown_730d69d7-7e0f-4b21-8ab8-725bcec1bfd3.html).
10. Write code to create log entries and send them to the Logging Application Block. For more information, see the topic [Populating and Raising Events from Code](test-markdown_3712145d-7fa5-4fd7-b9a7-ea2d018b5fc7.html) and [Populating a Log Message with Additional Context Information](test-markdown_62843eda-e525-4531-8d26-4efddd75ccef.html).

