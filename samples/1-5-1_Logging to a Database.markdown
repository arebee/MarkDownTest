---
Source File Name: 40-Logging.docx
AssetID: b010d39c-5196-439a-8d7c-92d6cbe1d892
Title: Logging to a Database
Order In ToC: 1-5-1
Output Filename: 1-5-1_Logging to a Database.markdown
---

#### Markdown Test ####
# Logging to a Database #
----------

<a name="logscenario_database" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>This scenario is one of several that describe the typical requirements when using the Logging Application Block in your applications. It describes the process for setting up the block to send log events to a database. The process involves configuring the block and performing other tasks to prepare your application. This topic acts as a reference to help you quickly set up the Logging Application Block to perform the required logging action.  

# Typical Goals #
You need to send logging information created as log entries to one or more sinks or targets, and the list of targets includes a database. You want to set up the Logging Application Block to use a suitable trace listener, configure the options and the log entry format, and configure the database to receive log entries.   

# Solution #
The high-level stages of the process that you should follow are to:  
+ Configure the database for logging.
+ Configure your application to use the Logging Application Block with a database trace listener.
+ Configure the properties of the database trace listener.
+ Configure the required filters and log sources for the Logging Application Block.
+ Add references to the Logging Application Block to your application and write code to send log entries to the block. 
The following steps describe the whole process in more detail.  
**To configure logging to a database**

1. In the folder where you installed the Enterprise Library (by default, this is EntLib5Src), open the subfolder Source\Blocks\Logging\Src\DatabaseTraceListener\Scripts in Windows® Explorer. This folder contains a SQL script named LoggingDatabase.sql and a command file named CreateLoggingDb.cmd that executes this script. The script creates a new database named **Logging**, and adds to it all of the tables and stored procedures required by the database trace listener to write log entries to a database.![](images/note.gif)By default, thedatabasetrace listener writes log entries into the local SQL Server® Express database. Before you execute the command file, you can specify a different database server by editing the command file to specify this server. For example, to use the default database server on a remote machine, you may use something likeMYSERVERNAME,tcp:&gt; **10.0.0.1**, or **myserver.mydomain.com**. 
If you already have a database that you want to use for logging, you can adapt the script to add the required tables and stored procedures to your existing database.
2. Execute the script to create the required objects in the target database. If you are using a remote server, configure the required accounts and permissions for the remote database so that the account you run your application under can access the stored procedures in the new **Logging** database. 
3. Add the Logging Application Block to your application configuration, and then add a database trace listener to the block configuration, as shown in the topic [Configuring Trace Listeners]({$finalDocSet}). 
4. Configure the properties of the database trace listener. Unless you changed the names of the stored procedures in the **Logging** database, you can leave the **Add Category Procedure** and **Write To Log Procedure** properties set to the default values of **AddCategory** and **WriteLog,** respectively. For more information about configuring the database trace listener, see the table of properties and descriptions in the topic [Trace Listener Properties]({$finalDocSet}).
5. In the Configuration Tool **Blocks** menu select the **Data Settings** section which opens the **Database Settings** and select a database that you have previously configured. This database item must specify the server containing the logging database. If required, set the **Severity Filter** property to specify the level of messages that will be logged. The default is **All**, but you can instead specify just a range of warning and error messages. For more information about configuring the database trace listener, see the table of properties and descriptions in the topic [Trace Listener Properties]({$finalDocSet}).  
6. Specify the required settings for the **Trace Output Options** property of the database trace listener. This allows you to include a range of information in the log entry, such as the date and time, the call stack, and the process ID. For more information about setting the **Trace Output Options** property of the database trace listener, see the table of possible values and descriptions in the topic [TraceOutputOptions Values]({$finalDocSet}).
7. Configure any trace sources you require. In general, you will add additional categories if you want to be able to assign log entries to more than the default General category. You may also want to change the settings for the **Logging Errors &amp; Warnings**, **Unprocessed Category**, and **All Events** special categories. For example, you may want to send entries for logging errors to the Windows Event Log or a disk file. For more information on configuring trace sources, see the topic [Configuring Trace Source Categories]({$finalDocSet}). 
8. Configure any log filters you require. You may want to filter log entries arriving at the Logging Application Block by category or by priority, or completely enable or disable logging. For more information on configuring log filters, see the topic [Configuring Logging Filters]({$finalDocSet}).
9. Add references to the Logging Application Block and other required assemblies to your application. For more details, see [Adding Application Code]({$finalDocSet}).
10. Write code to create log entries and send them to the Logging Application Block. For more information, see the topic [Populating and Raising Events from Code]({$finalDocSet}) and [Populating a Log Message with Additional Context Information]({$finalDocSet}).

