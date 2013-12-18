---
Source File Name: 40-Logging.docx
AssetID: 12cd1f02-219f-45a3-a711-bd50573de1e4
Title: Source Schema for the Logging Application Block
Order In ToC: 1-4-1-2-8
Output Filename: 1-4-1-2-8_Source Schema for the Logging Application Block.markdown
---

#### Markdown Test ####
# Source Schema for the Logging Application Block #
----------

This topic lists the elements and attributes used to configure the Logging Application Block. The configuration file has the following section-handler declaration.  

```other
&lt;configSections&gt;
&lt;section name="loggingConfiguration"
         type="Microsoft.Practices.EnterpriseLibrary.Logging.Configuration.LoggingSettings, 
               Microsoft.Practices.EnterpriseLibrary.Logging" /&gt;
&lt;/configSections&gt;
```

The section-handler declaration contains the name of the configuration settings section and the name of the section-handler class that processes configuration data in that section. The name of the configuration settings section is **loggingConfiguration**. The name of the section-handler class is **Microsoft.Practices.EnterpriseLibrary.Logging.Configuration.LoggingSettings**.  

# loggingConfiguration Element #
This element specifies the configuration of a Logging Application Block. This element is required.   
<a name="_Toc253064985" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes and Child Elements #
The following sections describe the attributes and child elements of the **loggingConfiguration** element. This element is required.  
<a name="_Toc253064986" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the **loggingConfiguration** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>defaultCategory</b></p></td><td><p>Send log entries to this category if no other category is specified. This attribute is required.</p></td></tr><tr><td><p><b>tracingEnabled</b></p></td><td><p>Specifies whether activity tracing is enabled. Possible values are <b>true</b> and <b>false</b>. The default is <b>true</b>. This attribute is optional.</p></td></tr><tr><td><p><b>logWarningsWhenNoCategoriesMatch</b></p></td><td><p>Specifies whether events should be sent to the errors special source if a log entry contains a category that is not specified in configuration. Possible values are <b>true</b> and <b>false</b>. The default is <b>true</b>. This attribute is optional.</p></td></tr><tr><td><p><b>revertImpersonation</b></p></td><td><p>Specifies whether to opt-out of impersonation-reverting. To opt-out of the impersonation-reverting default mode, set <b>revertImpersonation</b> to <b>false</b>. Support includes configuration support, and design time support. The default is <b>true</b>. This attribute is optional. </p></td></tr></table>
# logFilters Child Element #
The **logFilters** element is a child element of the **loggingConfiguration** element. It lists the filters that process the log entry before it is routed to the trace sources. This element is optional.  
<a name="_Toc253064988" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# add Child Element #
The **add** element is a child element of the **logFilters** element. The **add** element adds a log filter. This element is optional. There can be multiple **add** elements.  
<a name="_Toc253064989" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** add** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of the log filter. The name must be unique within the section. This attribute is required.</p></td></tr><tr><td><p><b>type</b></p></td><td><p>The type name of the class that implements the <b>ILogFilter</b> interface. This attribute is required.</p></td></tr><tr><td><p><b>enabled</b></p></td><td><p>Specifies whether logging is enabled or disabled for the application. This attribute applies to the <b>LogEnabledFilter</b> class. Possible values are <b>true</b> and <b>false</b>. This attribute is optional. </p></td></tr><tr><td><p><b>minimumPriority</b></p></td><td><p>Specifies the minimum priority a log entry must have to be logged. The attribute applies to the <b>PriorityFilter</b> class. It is an integer. If this attribute is omitted, there is no minimum. It is optional. </p></td></tr><tr><td><p><b>maximumPriority</b></p></td><td><p>Specifies the maximum priority a log entry can have to be logged. The attribute applies to the <b>PriorityFilter</b> class. It is an integer. If this attribute is omitted, there is no maximum. It is optional.</p></td></tr><tr><td><p><b>categoryFilterMode</b></p></td><td><p>Specifies whether the categories listed under <b>&lt;categoryFilters&gt;</b> are allowed or denied. This attribute applies to the <b>CategoryFilter</b> class. Possible values are <b>AllowAllExceptDenied</b> and <b>DenyAllExceptAllowed</b>. This attribute is required.</p></td></tr></table>
# categoryFilters Child Element #
The **categoryFilters** element is a child of the **logFilters** element. It lists the categories that are allowed or denied. This element is optional.  
<a name="_Toc253064991" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# add Child Element #
The **add** element is a child element of the **categoryFilters** element. The **add** element adds a category. This element is optional. There can be multiple **add** elements.  
<a name="_Toc253064992" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** add** element.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of the category that is allowed or denied. This attribute is a string. It is required.</p></td></tr></table>
# categorySources Child Element #
The **categorySources** element is a child of the **loggingConfiguration** element. It defines the trace sources that route log entries belonging to specific categories to trace listeners.  
<a name="_Toc253064994" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# add Child Element #
The **add** element is a child element of the **categorySources** element. The **add** element adds a trace source. This element is optional. There can be multiple **add** elements.  
<a name="_Toc253064995" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** add** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of the log entry category that this trace source accepts. The name must be unique within the section. This attribute is required.</p></td></tr><tr><td><p><b>switchValue</b></p></td><td><p>Sets the <b>Minimum Severity</b> (<b>SourceLevels</b>) property of the category. Specifies the severity level of events that are handled by this trace source. Possible values are <b>ActivityTracing</b>, <b>All</b>, <b>Critical</b>, <b>Error</b>, <b>Information</b>, <b>Off</b>, <b>Verbose</b>, and <b>Warning</b>. This attribute is required.</p></td></tr><tr><td><p><b>autoFlush</b></p></td><td><p>Specifies if the listener will flush its data to the target after every new entry is written to this source. The default value is <b>true</b>. When set to <b>false</b>, code can flush the entries when required.</p></td></tr></table><a name="_Toc253064996" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Remarks #
The meanings of the **switchValue** attribute values are as follows:  
+ **ActivityTracing**. Allows the Stop, Start, Suspend, Transfer, and Resume events through.
+ **All**. Allows all events through.
+ **Critical**. Allows only **Critical** events through. A critical event is a fatal error or application crash.
+ **Error**. Allows **Critical** and **Error** events through. An **Error** event is a recoverable error.
+ **Information**. Allows **Critical**, **Error**, **Warning**, and **Information** events through. An information event is an informational message.
+ **Off**. Does not allow any events through.
+ **Verbose**. Allows **Critical**, **Error**, **Warning**, **Information**, and **Verbose** events through. A **Verbose** event is a debugging trace.
+ **Warning**. Allows **Critical**, **Error**, and **Warning** events through.
For more information about these values, see [TraceEventType Enumeration](http://msdn2.microsoft.com/en-us/library/system.diagnostics.traceeventtype) in the .NET Class Framework Library on MSDN.  

# listeners Child Element (categorySources) #
This **listeners** element is a child of an **&lt;add&gt;** section for a **categorySources** element. The **listeners** element specifies the trace listeners (listed in the **listeners** child element of the **loggingConfiguration** section) that the log entries are routed to. This element is optional.  
<a name="_Toc253064998" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# add Child Element #
The **add** element is a child element of the **listeners** element. The **add** element adds a trace listener. This element is optional. There can be multiple **add** elements.  
<a name="_Toc253064999" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** add** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of a listener from the listeners section. This attribute is required.</p></td></tr></table>
# specialSources Child Element #
The **specialSources** element is a child of the **loggingConfiguration** element. It specifies the special trace sources that can process certain types of log entries. This element is required.  
<a name="_Toc253065001" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# errors Child Element #
The **errors** element is a child of the **specialSources** element. It specifies the error special trace source. This trace source receives events for errors and warnings that occur during the logging process. This element is required.  
<a name="_Toc253065002" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** errors** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of the trace source. The name must be unique within the section. This attribute is required.</p></td></tr><tr><td><p><b>switchValue</b></p></td><td><p>Sets the <b>Minimum Severity</b> (<b>SourceLevels</b>) property. Specifies the severity level of events that are handled by this trace source. Possible values are <b>ActivityTracing</b>, <b>All</b>, <b>Critical</b>, <b>Error</b>, <b>Information</b>, <b>Off</b>, <b>Verbose</b>, and <b>Warning</b>. This attribute is required.</p></td></tr></table><a name="_Toc253065003" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Remarks #
The **switchValue** attribute values are defined in the **categorySources** section of this topic.  

# listeners Child Element (errors) #
This **listeners** element is a child of the **errors** element. It specifies the trace listeners (listed in the **listeners** child element of the **loggingConfiguration** section) that a log entry is routed to. This element is optional.  
<a name="_Toc253065005" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# add Child Element #
The **add** element is a child of the **listeners** element. It adds a trace listener. This element is optional. There can be multiple **add** elements.  
<a name="_Toc253065006" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** add** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of a listener from the <b>listeners</b> section. This attribute is required.</p></td></tr></table>
# notProcessed Child Element #
The **notProcessed** element is a child of the **specialSources** element. It specifies the unprocessed special trace source. This trace source receives log entries that are included in categories that were not processed by a category-based trace source. This element is optional.  
<a name="_Toc253065008" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** notProcessed** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of the trace source. The name must be unique within the section. This attribute is required.</p></td></tr><tr><td><p><b>switchValue</b></p></td><td><p>Sets the <b>Minimum Severity</b> (<b>SourceLevels</b>) property. Specifies the severity level of events that are handled by this trace source. Possible values are <b>ActivityTracing</b>, <b>All</b>, <b>Critical</b>, <b>Error</b>, <b>Information</b>, <b>Off</b>, <b>Verbose</b>, and <b>Warning</b>. This attribute is required.</p></td></tr></table><a name="_Toc253065009" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Remarks #
The **switchValue** attribute values are defined in the **categorySources** section of this topic.  

# listeners Child Element (notProcessed) #
This **listeners** element is a child of the **notProcessed** element. It specifies the trace listeners (listed in the **listeners** child element of the **loggingConfiguration** section) that a log entry is routed to. This element is optional.  
<a name="_Toc253065011" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# add Child Element #
The **add** element is a child of the **listeners** element. It adds a trace listener (listed in the **listeners** child element of the **loggingConfiguration** section). This element is optional. There can be multiple **add** elements.  
<a name="_Toc253065012" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** add** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p><b>Attribute</b></p></th><th><p><b>Description</b></p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of a listener from the <b>listeners</b> section. This attribute is required.</p></td></tr></table>
# allEvents Child Element #
The **allEvents** element is a child of the **specialSources** element. It specifies the all events special trace source. This trace source receives all log entries, whether or not they have been processed by other trace sources. This element is optional.  
<a name="_Toc253065014" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** allEvents** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of the trace source. The name must be unique within the section. This attribute is required.</p></td></tr><tr><td><p><b>switchValue</b></p></td><td><p>Sets the <b>Minimum Severity</b> (<b>SourceLevels</b>) property. Specifies the severity level of events that are handled by this trace source. Possible values are <b>ActivityTracing</b>, <b>All</b>, <b>Critical</b>, <b>Error</b>, <b>Information</b>, <b>Off</b>, <b>Verbose</b>, and <b>Warning</b>. This attribute is required.</p></td></tr></table><a name="_Toc253065015" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Remarks #
The **switchValue** attribute values are defined in the **categorySources** section of this topic.  

# listeners Child Element (allEvents) #
This **listeners** element is a child of the **allEvents** element. It specifies the trace listeners (listed in the **listeners** child element of the **loggingConfiguration** section) that a log entry is routed to. This element is optional.  
<a name="_Toc253065017" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# add Child Element #
The **add** element is a child of the **listeners** element. It adds a trace listener. This element is optional. There can be multiple **add** elements.  
<a name="_Toc253065018" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** add** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of a listener from the <b>listeners</b> section. This attribute is required.</p></td></tr></table>
# listeners Child Element (loggingConfiguration) #
The **loggingConfiguration** section contains details of the listeners referenced in other **listeners** elements. The **listeners** child element of the **loggingConfiguration** element lists the trace listeners that the Logging Application Block can use with the application. This element is optional.  
<a name="_Toc253065020" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# add Child Element #
The **add** element is a child element of the **listeners** element. The **add** element adds a trace listener. This element is optional. There can be multiple **add** elements.  
<a name="_Toc253065021" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** add** element alphabetically by attribute name.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>addCategoryStoredProcedure</b></p></td><td><p>The name of the stored procedure that adds a category. It applies to the database trace listener. The default is <b>AddCategory</b>. This is required for this listener.</p></td></tr><tr><td><p><b>databaseInstanceName</b></p></td><td><p>The name of the database instance to use. It applies to the database trace listener. this is required for this listener.</p></td></tr><tr><td><p><b>fileName</b></p></td><td><p>The name of the file that receives the log entry. This attribute applies to the flat file trace listener, the XML trace listener, and to the rolling flat file trace listener. It is required for these listeners.</p></td></tr><tr><td><p><b>filter</b></p></td><td><p>Applies a filter that selects the level of message that it will detect. The valid values are <b>All</b> (the default), <b>Off</b>, <b>Critical</b>, <b>Error</b>, <b>Warning</b>, <b>Information</b>, <b>Verbose</b>, and <b>ActivityTracing</b>. The setting effectively means "the specified level and everything more important." For example, the <b>Warning</b> setting will detect warnings, errors, and critical events.</p></td></tr><tr><td><p><b>footer</b></p></td><td><p>Additional information contained in the file footer. This attribute is a string and it applies to the flat file trace listener. It is optional.</p></td></tr><tr><td><p><b>formatter</b></p></td><td><p>The name of the formatter that formats the <b>LogEntry</b> object into a string. This name must be included in the &lt;<b>formatters</b>&gt; section. This attribute applies to the<b> </b>flat file trace listener, the e-mail trace listener, the event log trace listener, the message queuing trace listener, the database trace listener, the rolling flat file trace listener, the XML trace listener, and the custom trace listener. This attribute is required for the message queuing trace listener but not for other trace listeners. </p></td></tr><tr><td><p><b>fromAddress</b></p></td><td><p>The address where the log entry originated. This attribute is an e-mail address and it applies to the e-mail trace listener. It is required for this listener.</p></td></tr><tr><td><p><b>header</b></p></td><td><p>Additional information contained in the file header. This attribute is a string and it applies to the flat file trace listener. It is optional.</p></td></tr><tr><td><p><b>listenerDataType</b></p></td><td><p>The type name of a class that derives from the <b>TraceListenerData</b> class. This attribute is required.</p></td></tr><tr><td><p><b>log</b></p></td><td><p>The name of the log where entries are written. This attribute is a string and it applies to the event log trace listener. It is optional.</p></td></tr><tr><td><p><b>machineName</b></p></td><td><p>The name of the computer on which to write log entries. This attribute is a string and it applies to the event log trace listener. It is optional.</p></td></tr><tr><td><p><b>messagePriority</b></p></td><td><p>Sets the priority of a log entry. This determines its priority while it is in transit to the queue, and its position in the destination queue. Possible values are <b>AboveNormal</b>, <b>High</b>, <b>Highest</b>, <b>Low</b>, <b>Lowest</b>, <b>Normal</b>, <b>VeryHigh</b>, and <b>VeryLow</b>. It applies to the <b>Message Queuing Trace Listener</b> class. This attribute is optional.</p></td></tr><tr><td><p><b>name</b></p></td><td><p>The name of the trace listener. This can be any name. This attribute is required.</p></td></tr><tr><td><p><b>queuePath</b></p></td><td><p>The path to the queue that the message queuing trace listener uses. This attribute is a message queuing path and it applies to the message queuing trace listener. It is required for this listener.</p></td></tr><tr><td><p><b>recoverable</b></p></td><td><p>Specifies whether the log entry is guaranteed to be delivered if there is a computer failure or network problem. Possible values are <b>true</b> and<b> false</b>. It applies to<b> </b>the message queuing trace listener. This attribute is optional.</p></td></tr><tr><td><p><b>rollFileExistsBehavior</b></p></td><td><p>The behavior that occurs when the roll file is created. It applies to the rolling flat file trace listener. Possible values are<b> Overwrite</b> (the default), which overwrites the existing file<b> </b>and<b> Increment</b>, which<b> </b>creates a new file. It names the file by incrementing the time stamp.<b> </b>For more information, see the section "Rolling Flat File Trace Listener" in the topic <hlink xlink:type="simple" xlink:show="new" xlink:actuate="onRequest" xlink:href="b45ee518-82b1-426c-b772-1e6c0fde455e.html">Trace Listener Properties</hlink>.</p></td></tr><tr><td><p><b>rollInterval</b></p></td><td><p>The time interval that determines when the file rolls over. It applies to the rolling flat file trace listener. Possible values are <b>None</b> (the default), <b>Minute</b>, <b>Hour</b>, <b>Day</b>, <b>Midnight</b>, <b>Week</b>, <b>Month</b>, and <b>Year</b>. This is required for this listener.</p></td></tr><tr><td><p><b>rollSize</b></p></td><td><p>The size in kilobytes that the file can reach before it rolls over. It applies to the rolling flat file trace listener. This is optional.</p></td></tr><tr><td><p><b>smtpPort</b></p></td><td><p>The SMTP port that receives e-mail messages. This attribute is an integer and it applies to the e-mail trace listener. It is optional.</p></td></tr><tr><td><p><b>smtpServer</b></p></td><td><p>The SMTP server used to send e-mail messages. This attribute is a string and it applies to the e-mail trace listener. It is optional.</p></td></tr><tr><td><p><b>source</b></p></td><td><p>The source name to use when writing to the log. This attribute is a string and it applies to the event log trace listener. It is required for this listener.</p></td></tr><tr><td><p><b>subjectLineEnder</b></p></td><td><p>The subject line suffix. This attribute is a string and it applies to the<b> </b>e-mail trace listener. It is optional.</p></td></tr><tr><td><p><b>subjectLineStarter</b></p></td><td><p>The subject line prefix. This attribute is a string and it applies to the e-mail trace listener. It is optional.</p></td></tr><tr><td><p><b>timeStampPattern</b></p></td><td><p>The format of the date that is appended to the name of the new file. It applies to the rolling flat file trace listener. This is required for this listener.</p></td></tr><tr><td><p><b>timeToBeReceived</b></p></td><td><p>The total time for a log entry to be received by the destination queue. It applies to the message queuing trace listener. This attribute is of type <b>Timespan</b>. It is optional.</p></td></tr><tr><td><p><b>timeToReachQueue</b></p></td><td><p>The maximum amount of time for the log entry to reach the queue. It applies to the message queuing trace listener. This attribute is of type<b> Timespan</b>. It is optional.</p></td></tr><tr><td><p><b>toAddress</b></p></td><td><p>The address where the log entry is sent. This attribute is an e-mail address and it applies to the e-mail trace listener. It is required for this listener.</p></td></tr><tr><td><p><b>traceOutputOptions</b></p></td><td><p>A property used by trace listeners that do not output to a text formatter to determine which options, or elements, should be included in the trace output. Possible values are <b>CallStack</b>, <b>DateTime</b>, <b>LogicalOperationStack</b>, <b>None</b>, <b>ProcessId</b>, <b>ThreadId</b>, and <b>Timestamp</b>. The default is <b>None</b>. For an explanation of the values, see the Remarks section. This is optional.</p></td></tr><tr><td><p><b>transactionType</b></p></td><td><p>Specifies the type of a Message Queuing transaction. Possible values are <b>Automatic</b>, <b>None</b>, and <b>Single</b>. It applies to the message queuing trace listener. This attribute is optional.</p></td></tr><tr><td><p><b>type</b></p></td><td><p>The type name of a class that derives from the <b>TraceListener</b> class. This attribute is required.</p></td></tr><tr><td><p><b>useAuthentication</b></p></td><td><p>Specifies whether the message was (or must be)<b> </b>authenticated before being sent. Possible values are <b>true</b> or <b>false</b>. It applies to the message queuing trace listener. This attribute is optional.<b> </b></p></td></tr><tr><td><p><b>useDeadLetterQueue</b></p></td><td><p>Specifies whether a copy of a message that could not be delivered should be sent to a dead letter queue. Possible values are <b>true</b> or <b>false</b>. It applies to the message queuing trace listener. This attribute is optional.</p></td></tr><tr><td><p><b>useEncryption</b></p></td><td><p>Specifies whether to make the message private. Possible values are <b>true</b> or <b>false</b>. It applies to the message queuing trace listener. This attribute is optional.</p></td></tr><tr><td><p><b>writeLogStoredProcedureName</b></p></td><td><p>The name of the stored procedure that writes the log entries. It applies to the database trace listener. The default is <b>WriteLog</b>. This is required for this listener.</p></td></tr><tr><td><p><b>asynchronous</b></p></td><td><p>The flag indicating whether the listener should be used asynchronously. The default is <b>false</b>.</p></td></tr><tr><td><p><b>asynchronousTimeout</b></p></td><td><p>The period of time to wait for an asynchronous trace listener to finish processing buffered asynchronous requests, or infinite (expressed as "-00:00:00.0010000"). </p></td></tr></table><a name="_Toc253065022" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="remarks" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Remarks #
The meanings of the values for the **traceOutputOptions** attribute are shown in the table in the topic [TraceOutputOptions Values](test-markdown_e0902ea8-47a8-465f-a7d4-6809f35f6bc8.html). The values for the **messagePriority **attribute are as follows, in descending order of priority: **Highest**, **VeryHigh**, **High**, **AboveNormal**, **Normal**, **Low**, **VeryLow**, and **Lowest**. For more information about these values, see [MessagePriority Enumeration](http://msdn2.microsoft.com/en-us/library/system.messaging.messagepriority.aspx) in the .NET Framework Class Library on MSDN.  
The meanings of the values for the **transactionType** attribute are as follows:  
+ **Automatic**. A transaction type used for Microsoft® Transaction Server (MTS) or COM+ 1.0 Services. If there is already an MTS transaction context, it will be used when sending or receiving the message. 
+ **None**. Operation will not be transactional. 
+ **Single**. Transaction type used for single internal transactions. 
For more information about these values, see [MessageQueueTransactionType Enumeration](http://msdn2.microsoft.com/en-us/library/system.messaging.messagequeuetransactiontype.aspx) in the .NET Framework Class Library on MSDN.  

# formatters Child Element #
The **formatters** element is a child of the **loggingConfiguration** element. This element specifies formatters that format **LogEntry** objects. Formatters are used by formatting-aware trace listeners. This element is optional.  
<a name="_Toc253065024" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# add Child Element #
The **add** element is a child element of the **formatters** element. The **add** element adds a formatter. This element is optional. There can be multiple **add** elements.  
<a name="_Toc253065025" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the** add** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>name</b></p></td><td><p>The name of a formatter. The name must be unique within the section. This attribute is required.</p></td></tr><tr><td><p><b>type</b></p></td><td><p>The type name of a class that implements the <b>ILogFormatter</b> interface. This attribute is required.</p></td></tr><tr><td><p><b>template</b></p></td><td><p>This contains a template, including special tokens, that will be used by a formatter. This attribute applies to the <b>TextFormatter</b> class. It is a string and it is required for this formatter.</p></td></tr></table>
# msmqDistributorSettings Element #
The **msmqDistributorSettings** element controls the configuration of the Logging Application Block's distributor service. This element is required if you want to use Message Queuing to distribute log entries.  
<a name="_Toc253065027" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Attributes #
The following table lists the attributes for the **msmqDistributorSettings** element.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Attribute</p></th><th><p>Description</p></th></tr><tr><td><p><b>msmqPath</b></p></td><td><p>The name of the message queue. The default is ".\Private$\entlib". This attribute is required.</p></td></tr><tr><td><p><b>queueTimerInterval</b></p></td><td><p>The time interval that controls how often the distributor polls the message queue for log entries. The interval is measured in milliseconds. The default is 1,000. This attribute is optional.</p></td></tr><tr><td><p><b>serviceName</b></p></td><td><p>The name of the distributor service. The default is "Enterprise Library Distributor Service". This attribute is required.</p></td></tr></table>
