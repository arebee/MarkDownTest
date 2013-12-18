---
Source File Name: 40-Logging.docx
AssetID: 96dff44d-fb3e-4c3d-b6e4-948d0f8ac4f1
Title: When Should I Use the Logging Block?
Order In ToC: 1-2
Output Filename: 1-2_When Should I Use the Logging Block-.markdown
---

#### Markdown Test ####
# When Should I Use the Logging Block? #
----------

<a name="intro_whentouse" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>If your applications have a requirement to log information to Windows Event Log, e-mail, a database, a message queue, or a file, you should consider using the Logging Application Block to provide this functionality. In particular, the Logging Application Block is useful if you need to filter logging messages based on category or priority, if you need to format the messages, or if you need to change the destination of the message without changing the application code. The Logging Application Block is also designed to be extensible and includes the facility to create custom formatters and trace listeners, which you can adapt to meet your application's logging requirements.   

# Scenarios for the Logging Application Block #
The Logging Application Block addresses the most common tasks that developers face when they write applications that require logging functionality. These tasks are arranged in scenarios. Each scenario gives an example of a real-world situation, such as populating and raising events from code, discusses the particular requirements of the situation, and shows the code that accomplishes the task.  
The goal of arranging these tasks according to scenarios is to provide context for the code. Instead of showing an isolated group of methods, with no sense of where they can best be used, the Logging Application Block uses scenarios to show code usage in situations that are familiar to many developers whose applications must use logging features.  
The scenarios are the following:  
+ Configuring the block for typical logging scenarios 
+ Populating and raising events from code 
+ Populating a log message with additional context information 
+ Tracing activities and propagating context information 
+ Checking filter status before constructing log messages.
For details about each scenario, see [Key Scenarios]({$finalDocSet}).  

# Benefits of the Logging Application Block #
The Logging Application Block simplifies application development by providing a small set of easy-to-use classes and methods that encapsulate many of the most common logging scenarios. These scenarios include the following:  
+ Populating and logging event information
+ Including context information in the event
+ Tracing application activities
Each task is handled in a consistent manner, abstracting the application code from the specific logging providers.   
The Logging Application Block provides a consistent interface for logging information to any destination. Your application code does not specify the destination for the information. Configuration settings determine whether the application block writes the logging information and the location for that information. This means that operators as well as developers can modify logging behavior without changing application code.    
The Logging Application Block helps with application development in a number of ways:   
+ It helps maintain consistent logging practices, both within an application and across the enterprise. 
+ It eases the learning curve for developers by using a consistent architectural model. 
+ It provides implementations that you can use to solve common application logging tasks. 
+ It is extensible, supporting custom implementations of components that format and write event information. 

# Limitations of the Logging Application Block #
The Logging Application Block formatters do not encrypt logging information, and trace listener destinations receive logging information as clear text. This means that attackers that can access a trace listener destination can read the information. You may wish to explore options for preventing unauthorized access to sensitive information. One approach is to use access control lists (ACLs) to restrict access to flat files. You can also create a custom formatter that encrypts log information. For information about how to create a custom formatter, see [Extending the Logging Application Block]({$finalDocSet}).  
