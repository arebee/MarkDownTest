---
Source File Name: 40-Logging.docx
AssetID: 0d425989-6830-4e25-9fcf-e4dc2db4501a
Title: Entering Configuration InformationUsing Declarative Configuration
Order In ToC: 1-4-1-2
Output Filename: 1-4-1-2_Entering Configuration InformationUsing Declarative Configuration.markdown
---

#### Markdown Test ####
# Using Declarative Configuration #
----------

This section explains how to configure the Logging Application Block declaratively. For an overview and example of configuring the block, see [Configuration Overview]({$finalDocSet}). For details of the schema for the Logging Application Block configuration, see [Source Schema for the Logging Application Block]({$finalDocSet}). You can also configure the block in code by using an alternate configuration source. For more information, see [Advanced Configuration Scenarios]({$finalDocSet}) and [Using the Fluent Configuration API]({$finalDocSet}).  

![](images/note.gif)Note:&gt; Run time changes to the configuration of the Logging Application Block are automatically detected after a short period, and the logging stack is updated. For details of using configuration mechanisms that you can update at run time, see [Updating Configuration Settings at Run Time]({$finalDocSet}).
**To add the Logging Application Block**

1. Open the configuration file. For more information, see [Configuring Enterprise Library]({$finalDocSet}).
2. Open the **Blocks** menu and then click **Add Logging Settings**.
3. This adds a default general category, the three standard special categories, a default event log listener, and a default text formatter to the configuration.
4. Click the chevron expander arrow in the **Logging Settings** section to view all settings for this section.
5. Click the expander arrow at the left of the general category, the special categories, the event log listener, or the text formatter to view and set their properties as described in the following topics.
After you add the Logging Application Block to the application configuration, you can configure some or all of the following features. You should perform these tasks in the following order:  
+ [Configuring Trace Listeners]({$finalDocSet})
+ [Configuring Formatters]({$finalDocSet})
+ [Configuring Trace Source Categories]({$finalDocSet})
+ [Configuring Logging Filters]({$finalDocSet})
+ [Configuring the Application Block]({$finalDocSet})

# Usage Notes #
+ If you are using the Logging Application Block with a Windows Communication Foundation (WCF) application, you must configure integration with WCF. For details, see [Configuring WCF Integration Trace Listeners]({$finalDocSet}).
+ The configuration files are not encrypted by default. A configuration file may contain sensitive information about connection strings, user IDs, passwords, database servers, and catalogs. You should protect this information against unauthorized read/write operations by using encryption techniques. For information about how to encrypt configuration files, see [Configuring Enterprise Library]({$finalDocSet}).

