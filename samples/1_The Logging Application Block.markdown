---
Source File Name: 40-Logging.docx
AssetID: 318c9d2f-1ff6-438b-8d74-0b17074f470d
Title: The Logging Application Block
Order In ToC: 1
Output Filename: 1_The Logging Application Block.markdown
---

#### Markdown Test ####
# The Logging Application Block #
----------

Developers frequently write applications that require logging functionality. Typically, these applications format and log information in response to application events. For example, they may be required to log information in response to unexpected conditions, such as an application exception, or failure to connect to a database. Developers also write code to trace application flow through components during the execution of an application use case or scenario. In addition, applications often need to write information locally and over a network. In some cases, you may need to collate events from multiple sources into a single location.   
The Enterprise Library Logging Application Block simplifies the implementation of common logging functions. You can use the Logging Application Block to write information to a variety of locations:  
+ The event log 
+ An e-mail message
+ A database 
+ A message queue 
+ A text file 
+ Custom locations using application block extension points
This section includes the following topics that will help you to understand and use the Logging Application Block:  
+ <a href="test-markdown_75d79ffd-f3cf-48e7-bcbe-03acedf87ac0.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">What Does the Logging Block Do?</a> This topic provides a brief overview that will help you to understand what the block can do, and explains some of the concepts and features it incorporates. It also provides a simple example of how to write code to use the block.
+ <a href="test-markdown_96dff44d-fb3e-4c3d-b6e4-948d0f8ac4f1.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">When Should I Use the Logging Block?</a> This topic will help you to decide if the block is suitable for your requirements. It explains the benefits of using the block, and any alternative techniques you may consider. It also provides details of any limitations of the block that may affect your decision to use it.
+ <a href="test-markdown_af68990c-87d7-4af5-bdb8-ad1f3d611075.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Developing Applications Using the Logging Application Block</a>. This topic explains how to use the Logging Application Block in your applications. It shows how to configure the application block to perform common tasks and how to add application code to the application block where required. 
+ <a href="test-markdown_33d9998d-fa92-40b4-be49-7e28a72bd22c.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Key Scenarios</a>. This topic demonstrates how to use the application block to perform the most common logging operations. 
+ <a href="test-markdown_47589a17-a0d7-4651-93e1-41b9bc975eb6.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Design of the Logging Application Block</a>. This topic explains the decisions that went into designing the application block and the rationale behind those decisions. 
+ <a href="test-markdown_5d44c59c-4981-431a-aa38-5f466d4586c7.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Extending and Modifying the Logging Application Block</a>. This topic explains how to extend the application block by creating your own custom trace listeners, log formatters, and log filters; and explains how to modify the source code. 
+ <a href="test-markdown_11aa70e5-da29-457d-8600-a7507ff538dd.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Deployment and Operations</a>. This topic explains how to deploy and update the application block's assemblies. 
<a name="_Toc253064939" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# More Information #
For more information about logging and managing other crosscutting concerns, see the following patterns &amp; practices guides:  
+ <a href="http://msdn.microsoft.com/en-us/library/dd673617.aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Microsoft Application Architecture Guide, 2nd Edition</a>
+ <a href="http://msdn.microsoft.com/en-us/library/ms229014(VS.80).aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Design Guidelines for Exceptions</a> 

