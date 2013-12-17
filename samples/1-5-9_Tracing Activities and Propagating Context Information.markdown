---
Source File Name: 40-Logging.docx
AssetID: 76ae5d88-2ad5-4d53-a727-c8f80807d6d1
Title: Tracing Activities and Propagating Context Information
Order In ToC: 1-5-9
Output Filename: 1-5-9_Tracing Activities and Propagating Context Information.markdown
---

#### Markdown Test ####
# Tracing Activities and Propagating Context Information #
----------

In some cases, you will need to log information at the start and end of a particular activity, including timing information. In addition, you can trace the progress of the activity at selected points in the application. Tracing allows you to associate all events between the start and end of an activity with an **ActivityID** property and a category. The **ActivityID** property allows you to correlate log entries that are written during the execution of an activity. You can use filters and categories to direct and control the information produced for an event that can occur in the context of any activity.     

# Typical Goals #
In this scenario, the goal is to log the beginning and end of an activity to a common category. The configuration settings for the category determine the destination of the log entries.  

# Solution #
Configure the application to use the Logging Application Block, specifying the category to be used when tracing activity start and end times. Use the **StartTrace** method of the **TraceManager** class (which creates an instance of the **Tracer **class) where you want the timed event to begin, specifying a configured category. When the **Tracer** instance is created, the activity start time will be logged using the specified category. The following code assumes you have retrieved a reference to an instance of the **TraceManager** class and stored it in a variable named **traceMgr**.  

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
              <tr><td colspan="2"><pre>using (traceMgr.StartTrace("Log Category"))
{
  // Perform processing to be timed here
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
              <tr><td colspan="2"><pre>Using traceMgr.StartTrace("Log Category")
  ' Perform processing to be timed here
End Using</pre></td></tr>
            </table>
          </span>
        </div>
      The end time of the activity is logged using the same category when the **Tracer** object returned from the **StartTrace** method is disposed. The automatic disposal behavior of the **using** statement causes the end time to be logged when the scope of the **using** statement ends.   

<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>The non-static facade named **TraceManager** replaces the direct instantiation of the static **Tracer** class approach that was the default in earlier versions of Enterprise Library. For information about the previous approach, see the online documentation for Enterprise Library 4.1, available on the <a href="http://msdn.microsoft.com/en-gb/library/dd203099.aspx">MSDN Web Site</a>.</td></tr></table><p /></div>
<a name="_Toc253065066" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Specifying and Displaying the Activity ID #
By default, the **StartTrace** method will generate a unique identifier for the activity as a Guid. You can use this activity identifier to correlate all log entries for particular execution of an activity.  

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
              <tr><td colspan="2"><pre>using (traceMgr.StartTrace("General"))
{
  ...
}</pre></td></tr>
            </table>
          </span>
        </div>
      
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="VisualBasicUsage">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>Visual Basic</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Using traceMgr.StartTrace("General")
  ...
End Using</pre></td></tr>
            </table>
          </span>
        </div>
      Alternatively, you can specify the activity identifier in the call to the **StartTrace** method, as shown here. However, this prevents the block from generating a different activity identifier for each executing instance of the code. You will generally only use this approach if you want to apply some particular application-specific activity identifier.  

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
              <tr><td colspan="2"><pre>using (traceMgr.StartTrace("General", new Guid("{12345678-1234-1234-1234-123456789ABC}")))
{
  ...
}</pre></td></tr>
            </table>
          </span>
        </div>
      
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="VisualBasicUsage">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>Visual Basic</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Using traceMgr.StartTrace("General", New Guid("{12345678-1234-1234-1234-123456789ABC}"))
  ...
End Using</pre></td></tr>
            </table>
          </span>
        </div>
      The application block uses the .NET Framework **CorrelationManager** class to maintain the activity identifier. It automatically adds the activity ID to each log entry, but this does not appear in the resulting message when you use the text formatter with the default template. To include the activity ID in the logged message, edit the template property in the configuration tools to include the token **{property(ActivityId)}**. Note that property names are case-sensitive in the template definition.  
<a name="_Toc253065067" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Propagation of Context Information #
Categories determine the destinations for the log entries and are used for filtering. The categories associated with a **Tracer** object remain in effect as long as that **Tracer** object remains in scope. This means that the categories specified in the **StartTrace** method call are included in the list of categories for any call to the **LogWriter.Write** method while that **Tracer** object is in scope. In the following code, the message written in the call to the **LogWriter.Write** method belongs to both the UI Events category and the Debug category. The example assumes you have created an instance of the **LogWriter** class and saved it in a variable named **myLogWriter** and created an instance of the **TraceManager** class and stored it in a variable named **traceMgr**.   

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
              <tr><td colspan="2"><pre>using (traceMgr.StartTrace("UI Events"))
{
  // The following message will be associated with the 
  // "UI Events" and "Debug" categories.
  myLogWriter.Write("My message", "Debug");
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
              <tr><td colspan="2"><pre>Using traceMgr.StartTrace("UI Events")
  ' The following message will be associated with the  
  ' "UI Events" and "Debug" categories.
  myLogWriter.Write("My message", "Debug")
End Using</pre></td></tr>
            </table>
          </span>
        </div>
      By using the Logging Application Block, developers can trace the progress of an activity through a series of method calls. You can create log entries that have a common activity identifier by instantiating nested **Tracer** objects that do not specify an activity identifier. The nested **Tracer** objects will create log entries with the same activity identifier of the parent **Tracer** object currently in scope, as shown in the following code.  

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
              <tr><td colspan="2"><pre>using (traceMgr.StartTrace("A"))
{
  // Log entries created here will belong to category "A".
  ...
  using (traceMgr.StartTrace("B"))
  {
    // Log entries created here will belongs to both category "A" and category "B".
    ...    
  }
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
              <tr><td colspan="2"><pre>Using traceMgr.StartTrace("A")
  ' Log entries created here will belong to category "A".
  ...
  Using traceMgr.StartTrace("B")
    ' Log entries created here will belongs to both category "A" and category "B".
    ...    
  End Using
End Using</pre></td></tr>
            </table>
          </span>
        </div>
      **LogEntry** instances created within the scope of a nested **Tracer** object belong to the category specified for the nested **Tracer** object as well as to all categories for parent **Tracer** objects. All log entries for this activity have the same activity identifier. In the following example, the **LogEntry** object is associated with the categories UI Events, Data Access Events, and Troubleshooting. The example assumes you have created an instance of the **LogWriter** class and saved it in a variable named **myLogWriter** and created an instance of the **TraceManager** class and stored it in a variable named **traceMgr**.  

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
              <tr><td colspan="2"><pre>using (traceMgr.StartTrace("UI Events"))
{
  using (traceMgr.StartTrace("Data Access Events"))
  {
    LogEntry logEntry = new LogEntry();
    logEntry.Categories.Add("Troubleshooting");
    myLogWriter.Write(logEntry);
  }
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
              <tr><td colspan="2"><pre>Using traceMgr.StartTrace("UI Events")
  Using traceMgr.StartTrace("Data Access Events")
    Dim logEntry As New LogEntry()
    logEntry.Categories.Add("Troubleshooting")
    myLogWriter.Write(logEntry)
  End Using
End Using</pre></td></tr>
            </table>
          </span>
        </div>
      For information about how to create **LogWriter** and **TraceManager** instances, see <a href="test-markdown_875469ce-1185-4690-9d1c-36d452bf6a4a.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Creating LogWriter and TraceManager Instances</a>.  


# Usage Notes #
The **TraceManager.StartTrace** method creates and returns a reference to a **Tracer** instance that performs the trace function. Multiple **Tracer** objects may exist at one time. However, they must be disposed in the reverse order in which they were created. The recommended approach is to use a **using** statement (**Using** in Visual Basic). This causes your **Tracer** objects to be disposed in the correct order.  
If you explicitly dispose your **Tracer** objects, dispose them in reverse order than they were created. The following scenario is **not** supported.  

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
              <tr><td colspan="2"><pre>Tracer tracer1 = traceMgr.StartTrace("Category1");
Tracer tracer2 = traceMgr.StartTrace("Category2");

// INCORRECT: Should dispose tracer2 before tracer1.
tracer1.Dispose(); 
tracer2.Dispose();</pre></td></tr>
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
              <tr><td colspan="2"><pre>Dim tracer1 As Tracer = traceMgr.StartTrace("Category1")
Dim tracer2 As Tracer = traceMgr.StartTrace("Category2")

' INCORRECT: Should dispose tracer2 before tracer1.
tracer1.Dispose() 
tracer2.Dispose()</pre></td></tr>
            </table>
          </span>
        </div>
      
