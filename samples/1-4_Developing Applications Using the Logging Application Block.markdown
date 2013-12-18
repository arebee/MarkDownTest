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
+ [Configuring the Logging Application Block]({$finalDocSet})
+ [Using the Distributor Service]({$finalDocSet})
+ [Adding Application Code]({$finalDocSet})
If you want to deliver log entries at a central location for processing, you can use the Logging Application Block with Message Queuing (also known as MSMQ) to allow you to do this. By configuring multiple applications to use the same message queue, you can deliver all the log entries to one place. For details of how to install and configure the Logging Application Block in this scenario, see [Using the Distributor Service]({$finalDocSet}).  
If you are using the Logging Application Block with a Windows Communication Foundation (WCF) application, you must configure integration with WCF. For details, see [Configuring WCF Integration Trace Listeners]({$finalDocSet}).  
All application blocks ship as binary assemblies and as source code. If you want to use the source code, you must compile it. To learn how to compile the Enterprise Library source code, see [Building Enterprise Library from the Source Code]({$finalDocSet}).   
This topic assumes you are using the application block in its original state, without extending it. (To learn how to add functionality, see [Extending and Modifying the Logging Application Block]({$finalDocSet}).)  

