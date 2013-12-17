---
Source File Name: C:\code\pandp\git\markdown\sampleDocx\40-Logging.docx
AssetID: cb1f81e5-c18e-4844-ba6e-3be859c2f947
Title: Logging Messages Asynchronously
Order In ToC: 1-5-6
Output Filename: 1-5-6_Logging Messages Asynchronously.markdown
---

#### Markdown Test ####
# Logging Messages Asynchronously #
----------

The ability to write log messages asynchronously is useful in scenarios where there is a high volume of messages or where performance is critical.  

# Typical Goals #
Typically, the logging block writes messages synchronously to the destinations it supports. In some scenarios, such as when you have a high volume of messages to write, you may want to write the messages asynchronously with the risk that you may lose some messages if the buffer overflows. Writing log messages asynchronously is typically used when you are writing log messages to disk files or to a database.  

# Solution #
Configure the application to use the Logging Application Block. Wrap an existing trace listener using the **AsynchronousTraceListenerWrapper** class. The following code sample shows this.  

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
              <tr><td colspan="2"><pre>var asyncDatabaseTraceListener =
  new AsynchronousTraceListenerWrapper(databaseTraceListener);</pre></td></tr>
            </table>
          </span>
        </div>
      You can use the asynchronous trace listener in the same way that you would use the wrapped listener in your code.  
The **AsynchronousTraceListenerWrapper** constructor has three optional parameters in addition to the wrapped listener.   
+ **ownsWrappedListener**. Indicates whether the wrapper should dispose the wrapped trace listener.
+ **bufferSize**. Size of the buffer for asynchronous requests.
+ **maxDegreeOfParallelism**. The maximum degree of parallelism for thread safe listeners.
+ **disposeTimeout**. The timeout for waiting to complete buffered requests when disposing. When **null** (the default) the timeout is infinite.
If the buffer overflows, you may lose some messages.  
