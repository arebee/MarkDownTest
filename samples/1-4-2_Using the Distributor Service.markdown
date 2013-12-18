---
Source File Name: 40-Logging.docx
AssetID: ef65d516-04b0-44c2-a750-4e15aab636fd
Title: Using the Distributor Service
Order In ToC: 1-4-2
Output Filename: 1-4-2_Using the Distributor Service.markdown
---

#### Markdown Test ####
# Using the Distributor Service #
----------

Applications must often send log entries from multiple sources to a common destination. The Logging Application Block takes advantage of Message Queuing (also known as MSMQ) to allow you to do this. By configuring multiple applications to use the same message queue, you can process log entries at a central location.   
To distribute log entries to a central destination, configure your application to write log entries to the message queuing trace listener. When the application sends a log entry to the Logging Application Block, it places the log entry on a Message Queuing queue. The distributor service runs as a Windows service on either the same computer as the application or on a remote computer. It polls the queue to see if there are any log entries on it. The polling interval is determined by configuration.   
If there are log entries on the queue, the distributor service uses an instance of the Logging Application Block to forward the messages to the trace listener(s). The trace listener(s) write the log entries to the destinations, such as an event log or a flat file.  
The distributor service requires that all log entries be formatted using the **BinaryLogFormatter** class. If the service cannot interpret the entry, it will log an error to the Application Event Log and shut down.   
The following schematic illustrates how multiple applications use the distributor service to send log entries to a central location.  

![](images/f231119e-4654-4e6e-8449-b1e3b4e3d3b1.png)  

Each instance of the Logging Application Block uses an instance of the message queuing trace listener (the **MsmqTraceListener** class) to send the log entries to a single destination queue. The distributor service polls the queue and uses another instance of the Logging Application Block to direct the log entries to the proper trace listeners. Note that the distributor service can run on a remote computer. The following sections describe installing and using the distributor service:  
+ <a href="#distrib_install" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Installing the Distributor Service</a>
+ <a href="#distrib_start" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Starting the Distributor Service</a>
+ <a href="#distrib_understand" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Understanding the serviceName Attribute</a>

# Installing the Distributor Service #
<a name="distrib_install" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The Enterprise Library solution files include the distributor service project. You can run the BuildLibrary.bat batch file to create the service executable, MsmqDistributor.exe. The CopyAssemblies batch file puts the file in the bin directory, together with all the other application block assemblies. The CopyAssemblies batch file also puts the MsmqDistributor.exe.config file into the bin directory. If you want to install more than one instance of the distributor service, see the section <a href="#distrib_understand" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Understanding the serviceName Attribute</a>.  
The following procedure installs the distributor service.  
**To install the distributor service**

1. Deploy the Logging Application Block, the Enterprise Library Core, the MSMQDistributor.exe file, and the MsmqDistributor.exe.config file to the computer that will read log entries from the message queue. That computer must have Message Queuing installed. The computer can be the same as the one that will run your application, but it can also be another computer. For information about installing Message Queuing, see [Planning, Installing, and Upgrading Message Queuing](http://technet.microsoft.com/en-us/library/cc780048(WS.10).aspx) on MSDN.
2. Deploy any components required by the trace listeners. For example, if you configure the distributor service to write log entries to the database trace listener, you must also deploy the Data Access Application Block to the computer that runs the distributor service. 
3. Create the message queue using the Message Queuing tools. Remember the name you assign to the message queue.
4. Edit the MsmqDistributor.exe.config file. You cannot use the configuration console to do this. Use a text editor to edit the file and change the **msmqPath** attribute to match the name you assigned in Step 3. The default name is ".\Private$\entlib".
5. Install the distributor service. From the command line, type **installutil -i msmqdistributor.exe**. (The installer tool, Installutil.exe, is located in the Framework directory. An example is C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727.)
6. When you run the installer tool, you will be prompted to enter a user name and a password for the account that the Message Queuing service runs under. For information about selecting an account with the correct service permissions, see the [Managing the Message Queuing service account](http://technet.microsoft.com/en-us/library/cc758845(WS.10).aspx) on MSDN. For general information on Message Queuing, see [Message Queuing Concepts](http://technet.microsoft.com/en-us/library/cc738910(WS.10).aspx).
After you install the distributor service, use the configuration console to configure the Logging Application Block that will use the service.  
To uninstall the distributor service, at the command line, type **installutil /u msmqdistributor.exe**.   

# Starting the Distributor Service #
<a name="distrib_start" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following procedure starts the distributor service.  
**To start the distributor service**

1. On the taskbar, click **Start**, click **Control Panel**, double-click **Administrative Tools**, and then double-click **Services**.
2. In the details panel, do one of the following: + Click the distributor service, and then click **Start** on the **Action** menu.
+ Right-click the distributor service, and then click **Start**.

Alternatively, you can start the distributor service from the command line. To start the distributor service from the command line, type the following at a command prompt.  

```other
net start "Enterprise Library Distributor Service"
```


![](images/note.gif)Note:&gt; You must either use the **net start** command or the **Services** item in Control Panel to start the distributor service. If you try to run the MsmqDistributor.exe file from the command line, you will receive a Windows Service Start Failure error message.

The distributor service does not detect changes to configuration information. You must restart the distributor service if you change the application block configuration. 
# Understanding the serviceName Attribute #
<a name="distrib_understand" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The MsmqDistributor.exe.config file contains a **serviceName** attribute. Do not change this name after you install the distributor service. If you do, you will not be able to uninstall the service because the system will try to uninstall a service with the new **serviceName** value.  
You also will be unable to restart the service after changing the **serviceName** attribute. When Windows restarts a service, it first initiates a service stop and then it initiates a service start. If you change the **serviceName** attribute, Windows will be unable to initiate the service stop because it will look for a service with the new name.  
You should use the **serviceName** attribute to run multiple instances of the distributor service on the same computer. The following procedure shows how to do this.  
**To install multiple instances of the distributor service**

1. Copy the Msmqdistributor.exe file and the MsmqDistributor.exe.config file to multiple directories.
2. Update the MsmqDistributor.exe.config file in each directory to have a unique service name. 
3. Run the Installutil tool for each copy. This will give each copy of the distributor service the same service name as the one in the corresponding configuration file. 
The distributor service uses the service name as the trace source when it logs messages to the event log. This log is always the Application Event Log.   

![](images/note.gif)Note:&gt; Duplicating the name of the distributor service will result in an error message during the installation.
