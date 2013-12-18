---
Source File Name: 40-Logging.docx
AssetID: a0ea0d8b-7675-48b8-9b5f-9d6d8e2382f0
Title: Configuring Trace Listeners
Order In ToC: 1-4-1-2-2
Output Filename: 1-4-1-2-2_Configuring Trace Listeners.markdown
---

#### Markdown Test ####
# Configuring Trace Listeners #
----------

Trace listeners receive log entries and write them to the appropriate destinations. The following procedure describes how to configure the trace listeners supplied with Enterprise Library and custom trace listeners that you create. There is also a specific procedure for configuring the two trace listeners that are required for integration with WCF. For more details of this procedure, see [Configuring WCF Integration Trace Listeners](test-markdown_4b216b82-b77d-41c3-bb14-cc1bf5d29db2.html).  

![](images/note.gif)Note:&gt; Trace listener logs have vulnerabilities and should be protected as appropriate, depending on whether the trace listener writes log entries to a file, database or MSMQ message queue. 

For trace listeners that write log entries to files you should protect the logs by using access control lists. This includes Flat File, Rolling flat file, and XML trace listener logs.

For trace listeners that write log entries to databases you should protect access to logs by using usernames and passwords. This includes the database trace listener.

For trace listeners that write log entries to MSMQ message queues you should allow only authorized users to read information from MSMQ. Protect the queue with relevant access control lists and follow any security considerations specific to MSMQ. This includes the message queuing trace listener.

# Configuring the Built-in Trace Listeners #
<a name="config_tracelisteners" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>**To configure trace listeners included with Enterprise Library**

1. Add a trace listener as described in [Configuration Overview](test-markdown_3a6ba613-78b1-4b24-b226-55e368e41554.html).
2. Click the property expander arrow in the trace listener** **item you want to configure. Each trace listener has an associated set of properties. For information about these properties, see the tables in the topic [Trace Listener Properties](test-markdown_b45ee518-82b1-426c-b772-1e6c0fde455e.html).![](images/note.gif)Note:&gt; If you are going to use the MSMQ distributor service, you must select the **Message Queuing Trace Listener **when adding a trace listener. For information about how to use the distributor service, see [Using the Distributor Service](test-markdown_ef65d516-04b0-44c2-a750-4e15aab636fd.html).
3. Set the **Formatter Name** property by clicking the formatter you want to use (if any) in the drop-down list. If you are configuring the message queuing trace listener to use it with the Message Queuing distributor service, you must select the **Binary Log Message Formatter**. 
4. If necessary, change the values of other properties. To do this, either type a new value or select an option in the drop-down list.
5. Repeat the procedure for each trace listener you add.

![](images/note.gif)Note:&gt; When you add a **System Diagnostics Trace Listener** and set the **Type Name** attribute, the type selector displays trace listener types that derive from the **System.Diagnostics.TraceListener** class. This means that the type selector displays the trace listener types that are a part of the **EnterpriseLibrary.Logging.TraceListeners** assembly as well as the trace listener types that are a part of the **System.Diagnostics** assembly. Only select types that belong to the **System.Diagnostics** assembly. You can use the Enterprise Library trace listeners that require either no parameters or one parameter. These trace listeners are the **Event Log Trace Listener**, and the **Flat File Trace Listener**.
# Configuring Custom Trace Listeners #
<a name="config_customtracelistener" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>**To configure a custom trace listener**

1. Select **Add Custom Trace Listener** as described in [Configuration Overview](test-markdown_3a6ba613-78b1-4b24-b226-55e368e41554.html). ![](images/note.gif)Note:&gt; The type selector will be empty and display no assemblies if no custom trace listeners are available. The **CustomTraceListenerData** attribute is required for custom trace listeners. 
2. Click the property expander arrow for the trace listener item to display its properties pane. 
3. Property values for custom listeners are configured as key/value pairs. To set these values, type the key and the value in the **Attributes** row of the property pane. As you add a key/value pair, a new empty row appears automatically. Click the cross icon next to a key/value pair to delete it.
4.  (Optional) Change the **Name** property. The default name is **Custom Trace Listener**.

