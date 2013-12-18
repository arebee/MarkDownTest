---
Source File Name: 40-Logging.docx
AssetID: 3a6ba613-78b1-4b24-b226-55e368e41554
Title: Configuration Overview
Order In ToC: 1-4-1-2-1
Output Filename: 1-4-1-2-1_Configuration Overview.markdown
---

#### Markdown Test ####
# Configuration Overview #
----------

To understand how you configure and use the Logging Application Block, you must be familiar with the way the block processes log entries. The topic [What Does the Logging Block Do?](test-markdown_75d79ffd-f3cf-48e7-bcbe-03acedf87ac0.html) provides a simplified overview of the process and will help you when you come to configure the block.  
The following steps illustrate the general process for configuring the Logging Application Block. It assumes you want to use two categories named Dev and Operations.   
1. Add the trace listeners you want to use to output the logging information by clicking on the plus sign icon in the **Logging Target Listeners** section of the **Logging Settings **configuration pane and clicking **Add Logging Target Listeners** to display the list of listeners. When you select a listener, it is added to the **Logging Target Listeners** section where you can then set its properties. ![](images/CA71C2EA01A5E4EBB0ED7FCB1CD54505.png)  

2. Add the formatters you want the trace listeners to use to format the output by clicking on the plus sign icon in the **Log Message Formatters** section of the **Logging Settings **configuration pane and clicking **Add Log Message Formatters** to display the list of formatters. When you select a formatter, it is added to the **Log Message Formatters** section where you can then set its properties. ![](images/c9db0dfe-8242-458d-8bd0-c1d0669db149.png)  

3. Set the **Formatter Name** property of each trace listener in the **Logging Target Listeners** section of the **Logging Settings **configuration pane to specify the appropriate formatter.
4. Add the category filters you want to use to the **Categories** section of the **Logging Settings **configuration pane by clicking on the plus sign icon in the **Categories** section of the **Logging Settings **configuration pane and clicking **Add Category**.** **A new category item is added to the **Categories** section where you can then set its properties. Click on its property expander arrow if the properties are not visible.
5. Add a reference to each of the trace listeners you want to use to output the logging information to each of the categories you created in the **Categories** section of the **Logging Settings** configuration by clicking the **Listeners** property plus sign icon and selecting a configured logging target listener. Each entry here is a reference to one of the trace listeners you previously configured in the **Trace Listeners** section. You can add more than one trace listener reference to each category filter you create. For example, you could specify the following:+ A log entry containing the category name Dev will go to an event log trace listener (so that it appears in Windows Event Log)
+ A log entry containing the category name Operations will go to an e-mail trace listener and to an XML trace listener. 

6. Add a reference to each of the trace listeners you want to use to output the logging information to the appropriate subsections of the **Special Categories** section of the **Logging Settings **configuration pane. For example, you may want to do the following: + Send the log entry to an e-mail trace listener when an error occurs within the logging system by adding a reference to this trace listener to the **Logging Errors &amp; Warnings** section.
+ Send the log entry to an event log trace listener when the log entry does not match any configured category by adding a reference to this trace listener to the **Unprocessed Category** section.
+ Send all log entries, irrespective of category or content, to a Flat File trace listener by adding a reference to this trace listener to the **All Events** section.

7. Set the remaining properties for each category or special filter. Specify the logging level, such as **Critical**, **Error**, **Warning**, or **All**, by setting the **Minimum Severity** property for each category filter. ![](images/1C76D787B795608F7BFFBBCDEA6AC14A.png)  

8. Finally, add any log filters you want to use to the **Logging Filters** section of the **Logging Settings **configuration pane by clicking on the plus sign icon in the **Logging Filters** section of the **Logging Settings **configuration pane and clicking **Add Logging Filters**.** **The filter type you select is added to the **Logging Filters** section where you can then set its properties. Click on its property expander arrow if the properties are not visible. For example, you could use a category filter to block all logging operations except those that specify one of the two categories named Dev or Operations.![](images/162b49c9-ca43-4395-8936-eecef3ac4c33.png)  

For specific details of how to configure each section of the Logging Application Block configuration, see [Using Declarative Configuration](test-markdown_0d425989-6830-4e25-9fcf-e4dc2db4501a.html).  

