---
Source File Name: 40-Logging.docx
AssetID: 875469ce-1185-4690-9d1c-36d452bf6a4a
Title: Creating LogWriter and TraceManager Instances
Order In ToC: 1-4-3-1
Output Filename: 1-4-3-1_Creating LogWriter and TraceManager Instances.markdown
---

#### Markdown Test ####
# Creating LogWriter and TraceManager Instances #
----------

Before you can use the features of the Logging Application Block in your application you must create an instance of the **LogWriter** class in your code and optionally an instance of the **TraceManager** class. The **LogWriter** class contains the overloaded implementations of the **Write** method that you will use to write log messages.  
When you create an instance of the **LogWriter** class you must populate it with details of your logging configuration; this configuration is typically defined in your application configuration file. The following code sample shows how to load the configuration settings from the configuration file and create a **LogWriter** instance.  

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
              <tr><td colspan="2"><pre>LogWriterFactory logWriterFactory = new LogWriterFactory();
LogWriter defaultWriter = logWriterFactory.Create();</pre></td></tr>
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
              <tr><td colspan="2"><pre>Dim logWriterFactory As New LogWriterFactory()
Dim defaultWriter As LogWriter = logWriterFactory.Create()</pre></td></tr>
            </table>
          </span>
        </div>
      You can use the **LogWriter** instance to configure a **TraceManager** instance. The **TraceManager** class includes a constructor that takes a **LogWriter** instance as a parameter.   
For more information about creating Enterprise Library application block instances, see <a href="test-markdown_6453869a-c2bb-4494-9095-ea97d328ee94.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Creating Application Block Objects</a>.  

