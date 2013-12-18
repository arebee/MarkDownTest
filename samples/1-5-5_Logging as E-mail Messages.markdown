---
Source File Name: 40-Logging.docx
AssetID: c28f7cd6-7933-47ba-8d30-ac4ecaa96a52
Title: Logging as E-mail Messages
Order In ToC: 1-5-5
Output Filename: 1-5-5_Logging as E-mail Messages.markdown
---

#### Markdown Test ####
# Logging as E-mail Messages #
----------

This scenario is one of several that describe the typical requirements when using the Logging Application Block in your applications. It describes the process for setting up the block to send log events as e-mail messages. The process involves configuring the block and performing other tasks to prepare your application. This topic acts as a quick reference to help you rapidly set up the Logging Application Block to perform the required logging action.  

# Typical Goals #
You need to send logging information created as log entries to one or more sinks or targets, and the list of targets includes sending log entries as e-mail messages. You want to set up the Logging Application Block to use a suitable trace listener, and configure the e-mail server and message options you require.   

# Solution #
The high-level stages of the process that you should follow are to:  
+ Identify the mail server (if any) your will use to send the e-mail messages, and other details such as e-mail addresses and the subject text. 
+ Configure your application to use the Logging Application Block with an e-mail trace listener.
+ Configure the properties of the e-mail trace listener.
+ Configure the required filters and log sources for the Logging Application Block.
+ Add references to the Logging Application Block to your application and write code to send log entries to the block. 
The following steps describe the whole process in more detail.  
**To configure logging as e-mail messages**

1. Add the Logging Application Block to your application configuration, and then add an e-mail trace listener to the block configuration, as shown in the topic [Configuring Trace Listeners]({$finalDocSet}). 
2. Configure the properties of the e-mail trace listener. Many of these properties define how the e-mail trace listener will interact with your mail server and send messages. You must specify the From and To addresses as the values of the **From Address** and **To Address** properties. You can also edit the default values for the properties that identify your mail server. These are the **Smtp Port** property (the default port is 25) and the **Smtp Server** property (the default is the local SMTP service on 127.0.0.1). In addition, you can specify a prefix and suffix for the message subject as the values of the **Subject Line Prefix** and **Subject Line Suffix** properties. The block will create a suitable message subject from the log entry content, and include these values. If you need to specify credentials to access your e-mail server or need to use the SSL protocol, set the **Authentication Mode**, **Authentication User Name**, **Authentication Password**, and **Use SSL** properties. For more information about configuring the e-mail trace listener, see the table of properties and descriptions in the topic [Trace Listener Properties]({$finalDocSet}).  
3. Select the text formatter for the **Formatter Name** property of the e-mail trace listener, or add a new text formatter to the configuration and select this. If required, edit the **Template** property of the text formatter to specify the actual text format you want. You can change the text of the log message, and insert placeholders for the values exposed by the log entry. For more details, see [Configuring Formatters]({$finalDocSet}).
4. Set the **Severity Filter **property of the e-mail trace listener to specify the level of messages that will be logged. The valid values are **All** (the default), **Off**, **Critical**, **Error**, **Warning**, **Information**, **Verbose**, and **ActivityTracing**. The setting effectively means "the specified level and everything more important." For example, the **Warning** setting will detect warnings, errors, and critical events.
5. Configure any trace sources you require. In general, you will add additional categories if you want to be able to assign log entries to more than the default **General** category. You may also want to change the settings for the **Logging Errors &amp; Warnings**, **Unprocessed Category**, and **All Events** special trace sources. For example, you may want to send entries for logging errors to the Windows Event Log or a disk file. For more information on configuring trace sources, see the topic [Configuring Trace Source Categories]({$finalDocSet}). 
6. Configure any log filters you require. You may want to filter log entries arriving at the Logging Application Block by category or by priority, or completely enable or disable logging. For more information on configuring log filters, see the topic [Configuring Logging Filters]({$finalDocSet}).
7. Add references to the Logging Application Block and other required assemblies to your application. For more details, see [Adding Application Code]({$finalDocSet}).
8. Write code to create log entries and send them to the Logging Application Block. For more information, see the topic [Populating and Raising Events from Code]({$finalDocSet}) and [Populating a Log Message with Additional Context Information]({$finalDocSet}).
