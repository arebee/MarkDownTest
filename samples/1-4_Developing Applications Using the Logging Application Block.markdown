---
Source File Name: 40-Logging.docx
AssetID: af68990c-87d7-4af5-bdb8-ad1f3d611075
Title: Developing Applications Using the Logging Application Block
Order In ToC: 1-4
Output Filename: 1-4_Developing Applications Using the Logging Application Block.markdown
---

#### Markdown Test ####
# Developing Applications Using the Logging Application Block #
----------

This topic describes how to develop applications using the Logging Application Block. It explains how to configure the application block to perform particular tasks and how to use the application block according to particular application scenarios such as populating and raising events from code. It includes the following topics:  
+ <a href="test-markdown_8f4b85fc-1789-488a-ab1e-e8b67459d95d.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Configuring the Logging Application Block</a>
+ <a href="test-markdown_ef65d516-04b0-44c2-a750-4e15aab636fd.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Using the Distributor Service</a>
+ <a href="test-markdown_730d69d7-7e0f-4b21-8ab8-725bcec1bfd3.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Adding Application Code</a>
If you want to deliver log entries at a central location for processing, you can use the Logging Application Block with Message Queuing (also known as MSMQ) to allow you to do this. By configuring multiple applications to use the same message queue, you can deliver all the log entries to one place. For details of how to install and configure the Logging Application Block in this scenario, see <a href="test-markdown_ef65d516-04b0-44c2-a750-4e15aab636fd.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Using the Distributor Service</a>.  
If you are using the Logging Application Block with a Windows Communication Foundation (WCF) application, you must configure integration with WCF. For details, see <a href="test-markdown_4b216b82-b77d-41c3-bb14-cc1bf5d29db2.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Configuring WCF Integration Trace Listeners</a>.  
All application blocks ship as binary assemblies and as source code. If you want to use the source code, you must compile it. To learn how to compile the Enterprise Library source code, see <a href="test-markdown_e13a03ac-387f-4300-bff0-f97c33628f47.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Building Enterprise Library from the Source Code</a>.   
This topic assumes you are using the application block in its original state, without extending it. (To learn how to add functionality, see <a href="test-markdown_5d44c59c-4981-431a-aa38-5f466d4586c7.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Extending and Modifying the Logging Application Block</a>.)  

