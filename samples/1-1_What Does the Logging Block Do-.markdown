---
Source File Name: 40-Logging.docx
AssetID: 75d79ffd-f3cf-48e7-bcbe-03acedf87ac0
Title: What Does the Logging Block Do?
Order In ToC: 1-1
Output Filename: 1-1_What Does the Logging Block Do-.markdown
---

#### Markdown Test ####
# What Does the Logging Block Do? #
----------

Although the process of creating and writing log entries is relatively simple, the number of options available (such as the many logging targets and the ability to filter entries) means that the underlying structure of the block and the options available for using it can seem complex. The following schematic shows how the main types of object in the block work together to provide flexibility when creating and writing log entries.  

!(images/fc5b4b63-76c0-4b6a-a61d-0c41c447d126.png)  

The five main types of objects are:  
+ **Log Writer**. The log writer is the main entry point for creating log entries and writing them to your chosen logging targets. It creates an instance of a log entry containing the information to be logged, and interacts with the other objects that filter the log entry, assign it to one or more categories, format it, and dispatch it to the appropriate targets. 
+ **Log Filters**. Log filters can block or allow a log entry based on a number of features. Each log entry is assigned to one or more categories (the default is the General category), and the category log filter can use these categories to pass or block a log entry. In addition, two special log filters can block all logging, or block log entries with a priority lower than a specified value. You define the categories, priorities, and the settings for the log filters in the configuration for the block.
+ **Trace Sources**. Trace sources are effectively a set of buckets into which the block places all log entries that have not been blocked by a log filter. You use these buckets to define where log entries will be dispatched to—you can think of them as being the source of the log entries that will actually be dispatched to the target destinations. There are two basic types of trace sources:+ There is a trace source for each category you define in the configuration of the block. These are called **Category Sources**.
+ There are three built-in trace sources: one that receives all log entries, one that receives log entries when an error occurs during processing or dispatching of the log entry, and one that receives all log entries that do not match any configured category. These are called **Special Sources**.

+ **Trace Listeners**. Trace listeners represent the targets for your log entries, and you configure one for each type of target (such as the Windows® Event Log, a disk file, and an e-mail message) to which you want to send the log entries. Trace listeners listen for log entries arriving in the trace source buckets, format each log entry as required, and dispatch it to the target configured for that trace source. Your configuration maps each trace source (each category source you define plus the three special sources) to one or more trace listeners. The important point to note here is that this allows you to dispatch each log entry to zero, one, or more targets (such as sending it as e-mail as well as writing it to the Windows Event Log).
+ **Log Formatters**. Each trace listener you add to your configuration can use a log formatter to convert the data in the log entry from a series of properties into format suitable for sending to the log target. The block contains a text formatter that you can configure with trace listeners that dispatch log entries to targets such as disk files, e-mail, or Windows Event Log; and a binary formatter that serializes the log entry data into a format suitable for transmission to targets such as Windows Message Queuing (MSMQ).  The text formatter is configurable so that you can modify the format and content of the text message, including using placeholders for the values of the properties of the log entry.

# The Logging Process Sequence #
Although the logging process should be clear to you now from the description of the objects used, the following describes the process step by step to help you understand how it relates to the set of objects it uses.  
1. Your application submits information that the Logging Application Block should log either by using the **LogWriter** class to automatically create a log entry or by directly creating an instance of the **LogEntry** class and populating it with the information to log. The log entry can define a set of categories that map to the categories defined in the configuration. This mapping controls the processes that the block applies to the log entry (the filters and sources that it uses).  
2. The log writer passes the log entry through the log filters that you define in the configuration. These filters can block the log entry based on its priority, category name, or when logging is not enabled.
3. If the log filters do not block the log entry, the log writer retrieves the appropriate trace sources. The trace sources you can use consist of a set of category sources that you create and configure, and three special sources that you can use to ensure that the  block will record all log entries (for example, if there is an error within the logging system or if the log entry does not match any configured category). 
4. You can configure one or more trace listeners for each of the trace sources. In other words, you can configure a specific set of trace listeners for each category that the message might contain, and for each of the three special sources that might handle the message. The log writer will pass the log entry to the matching trace listeners you specify. 
5. The trace listener then uses a log formatter to transform the information in the log entry into an appropriate format, such as formatted text or binary, and writes the result to the output specific to the type of trace listener. Depending on the trace listener type, the output can go to a file, a database, Message Queuing (also known as MSMQ), or as an e-mail message.
For an overview of the process of setting up logging for different types of sink (or logging target), see <a href="test-markdown_33d9998d-fa92-40b4-be49-7e28a72bd22c.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Key Scenarios</a>.  

# Example Application Code #
<a name="intro_examplecode" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following code shows how to populate and raise an event in your application that writes the log entry to the target or targets you configure for the block**.** The **LogEntry** object has a priority of 2 and belongs to both the **Trace** and **UI Events** categories. This example assumes that you have created an instance of the **LogWriter** class in your application.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>public void ExampleScenario(LogWriter myLogWriter)
{
  LogEntry logEntry = new LogEntry();

  logEntry.EventId = 100;
  logEntry.Priority = 2;
  logEntry.Message = "Informational message";
  logEntry.Categories.Add("Trace");
  logEntry.Categories.Add("UI Events");

  myLogWriter.Write(logEntry);
}</pre></td></tr>
            </table>
          </span>
        </div>
      
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="VisualBasicUsage">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>Visual Basic</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Public Sub ExampleScenario(ByVal myLogWriter As LogWriter)
  Dim logEntry As New LogEntry()

  logEntry.EventId = 100
  logEntry.Priority = 2
  logEntry.Message = "Informational message"
  logEntry.Categories.Add("Trace")
  logEntry.Categories.Add("UI Events")

  myLogWriter.Write(logEntry)

End Sub</pre></td></tr>
            </table>
          </span>
        </div>
      For information about resolving Enterprise Library objects in your applications, see <a href="test-markdown_bfd186b8-9a32-477a-bee7-14742ba1ca42.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Creating and Referencing Enterprise Library Objects</a>.  

