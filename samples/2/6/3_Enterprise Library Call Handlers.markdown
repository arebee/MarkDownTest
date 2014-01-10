---
Source File Name: 75-Interception.docx
AssetID: 969b6f02-4da3-41d1-8527-c9e0009d1632
Title: Enterprise Library Call Handlers
Order In ToC: 2\6\3
Output Filename: 2\6\3_Enterprise Library Call Handlers.markdown
---

#### Markdown Test ####
# Enterprise Library Call Handlers #
----------


&gt; ![](/images/note.gif)#!155CharTopicSummary!#:
&gt; 
This section discusses some of the common scenarios for using the Policy Injection Application Block call handlers.
The [patterns &amp; practices Enterprise Library](http://msdn.microsoft.com/entlib/) includes a set of call handlers and the equivalent call handler attributes designed for use with the Unity **PolicyInjectionBehavior**. This section discusses some of the common scenarios for using the Policy Injection Application Block call handlers. It examines the following common scenarios where the block can simplify and accelerate application development:  
+ <a href="#scenarios_logging" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Logging Method Invocation and Property Access</a>
+ <a href="#scenarios_exceptions" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Handling Exceptions in a Structured Manner</a>
+ <a href="#scenarios_validation" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Validating Parameter Values</a>
+ <a href="#scenarios_authorization" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Authorizing Method and Property Requests</a>
+ <a href="#scenarios_performance" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Measuring Target Method Performance</a>

# Logging Method Invocation and Property Access #
<a name="scenarios_logging" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Enterprise applications usually provide rich and detailed information about their operation in order to help administrators and operators measure activity, audit actions that take place, and detect unusual behavior, errors, and failures. Enterprise Library provides a handler that takes advantage of the features exposed by the Enterprise Library Logging Application Block. These features allow developers and administrators to configure logging to the Windows Event Log, e-mail messages, databases, message queue systems, text files, Windows Management Instrumentation (WMI) events, or custom locations. For more details about logging method invocations and property accesses, see [The Logging Handler](test-markdown_e7d4bacf-a864-4a50-b7c3-88acec5c4d7d.html).  

# Handling Exceptions in a Structured Manner #
<a name="scenarios_exceptions" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>All applications should handle errors in order to protect the application and to provide information for users, operators, administrators, and developers. Within .NET Framework-based applications, the most common scenario is handling exceptions raised by the application code, executing suitable recovery tasks where practicable, and raising the exception to the calling code or user interface.  
Enterprise Library provides a handler that takes advantage of the features exposed by the Enterprise Library Exception Handling Application Block. These features allow developers to create a consistent strategy for processing exceptions that occur in all architectural layers of an enterprise application. They also allow administrators to define and maintain exception handling policies, and they support the following:  
+ Logging exception information.
+ Hiding sensitive information by replacing the original exception with another exception.
+ Wrapping exceptions inside a new exception to maintain contextual information.
+ Combining these tasks so that, for example, the application block will log the information from an exception and then replace the original exception with another.
For more details about structured exception handling using policy injection, see [The Exception Handling Handler](test-markdown_d874dee7-1158-4cd7-900a-d592b5da5e69.html).  

# Validating Parameter Values #
<a name="scenarios_validation" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Applications should protect against errors caused by invalid or out-of-range values provided as input to the user interface or applied to methods or properties within the code. Writing validation code is always an onerous task and there is always the chance that you will repeat an action or overlook some unexpected situation. Enterprise Library provides a handler that takes advantage of the features exposed by the Validation Application Block; this minimizes the code developers have to create and it allows administrators to modify the validation policies through configuration if required.  
For more details about applying validation to class members using policy injection, see [The Validation Handler](test-markdown_ad452cb9-20c7-4db2-9801-73417714f46c.html).  

# Authorizing Method and Property Requests #
<a name="scenarios_authorization" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Distributed applications frequently use services and access classes that reside in separate security contexts. If the current user does not have permission to access a target class instance, an exception occurs that may be difficult to diagnose. Administrators and operators often find that a large proportion of application installation, set up, and run-time maintenance involves setting appropriate permissions and authorizing users and services.  
Enterprise Library provides a handler that will check the authorization status of the requesting user or thread against the target class permissions before calling the target method or accessing the target property. The authorization handler takes advantage of the features exposed by the Enterprise Library Security Application Block, using an authorization provider defined in the application configuration.  
For more details about authorizing requests to members of a class using policy injection, see [The Authorization Handler](test-markdown_f27ca9a4-3284-4917-91b9-f2b8c73f24f0.html).  

# Measuring Target Method Performance #
<a name="scenarios_performance" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Enterprise Library contains instrumentation that allows developers, administrators, and operators to monitor the performance of the application blocks they use in their applications. In most cases, developers will also include instrumentation, such as performance counters, within their code to support monitoring of classes and resources used by the application. Enterprise Library and Unity provide additional and easy-to-use features for monitoring performance of target classes in the form of a handler that measures the time taken for the request to pass through the handler pipeline to the target class instance, and back to the application.   
One such feature is part of the logging handler; it requires only the addition of this handler as the first in the policy pipeline. The logging handler takes advantage of the features exposed by the Enterprise Library Logging Application Block, and it exposes the call duration as a property of the **TraceLogEntry** class. For more details about how the **CallTime** property exposed by the logging handler can provide performance measurement for individual methods, see [The Logging Handler](test-markdown_e7d4bacf-a864-4a50-b7c3-88acec5c4d7d.html).  
Another option for measuring the performance of the target methods is to use the performance counters handler. This handler increments a number of performance counters that provide detailed performance information, including the average execution time for the method and the number of times the method is called. For more information, see [The Performance Counter Handler](test-markdown_7f7053e1-9db6-433c-878f-b8a41b1d2a49.html).  

# More Information #
To understand how call handlers and the interception mechanism work, and how you can configure interception and policy injection, see the following topics :  
+ [Using Interception and Policy Injection](test-markdown_7a2c7fa6-28c2-479e-8df9-b4651824eb94.html)
+ [Configuring Unity](test-markdown_62fd666c-08c5-424a-b484-9e0b87994997.html)
+ [Attribute-Driven Policies](test-markdown_456aac54-4ba3-4904-adae-36fb5227fabc.html)
+ [The Authorization Handler](test-markdown_f27ca9a4-3284-4917-91b9-f2b8c73f24f0.html)
+ [The Exception Handling Handler](test-markdown_d874dee7-1158-4cd7-900a-d592b5da5e69.html)
+ [The Logging Handler](test-markdown_e7d4bacf-a864-4a50-b7c3-88acec5c4d7d.html)
+ [The Performance Counter Handler](test-markdown_7f7053e1-9db6-433c-878f-b8a41b1d2a49.html)
+ [The Validation Handler](test-markdown_ad452cb9-20c7-4db2-9801-73417714f46c.html)
+ [Extending and Modifying Unity](test-markdown_13f11174-8fd1-4406-8bc7-9da9c762811d.html)

