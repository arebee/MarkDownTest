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

This section explains how to configure the Logging Application Block declaratively. For an overview and example of configuring the block, see [Configuration Overview](test-markdown_3a6ba613-78b1-4b24-b226-55e368e41554.html). For details of the schema for the Logging Application Block configuration, see [Source Schema for the Logging Application Block](test-markdown_12cd1f02-219f-45a3-a711-bd50573de1e4.html). You can also configure the block in code by using an alternate configuration source. For more information, see [Advanced Configuration Scenarios](test-markdown_d0f735b9-e96f-4dcd-a16e-abe2d87cc30a.html) and [Using the Fluent Configuration API](test-markdown_d935824f-a38b-4a3d-8a86-76d279a6e761.html).  

![](images/note.gif)Note:&gt; Run time changes to the configuration of the Logging Application Block are automatically detected after a short period, and the logging stack is updated. For details of using configuration mechanisms that you can update at run time, see [Updating Configuration Settings at Run Time](test-markdown_1b2fd1e5-51a5-4f55-b0eb-cc830ca93b21.html).
**To add the Logging Application Block**

1. Open the configuration file. For more information, see [Configuring Enterprise Library](test-markdown_05ae7aff-85e4-46b5-8401-c4df81a2ea00.html).
2. Open the **Blocks** menu and then click **Add Logging Settings**.
3. This adds a default general category, the three standard special categories, a default event log listener, and a default text formatter to the configuration.
4. Click the chevron expander arrow in the **Logging Settings** section to view all settings for this section.
5. Click the expander arrow at the left of the general category, the special categories, the event log listener, or the text formatter to view and set their properties as described in the following topics.
After you add the Logging Application Block to the application configuration, you can configure some or all of the following features. You should perform these tasks in the following order:  
+ [Configuring Trace Listeners](test-markdown_a0ea0d8b-7675-48b8-9b5f-9d6d8e2382f0.html)
+ [Configuring Formatters](test-markdown_8b4b7563-0062-4690-bfc2-df37f15b2d35.html)
+ [Configuring Trace Source Categories](test-markdown_9301547d-44c4-490c-91a0-b63e86e4b6a2.html)
+ [Configuring Logging Filters](test-markdown_ac913544-cc72-4de9-b916-f9d85d473685.html)
+ [Configuring the Application Block](test-markdown_2b5c86d6-f126-4425-9bce-731bb2fcd52d.html)

# Usage Notes #
+ If you are using the Logging Application Block with a Windows Communication Foundation (WCF) application, you must configure integration with WCF. For details, see [Configuring WCF Integration Trace Listeners](test-markdown_4b216b82-b77d-41c3-bb14-cc1bf5d29db2.html).
+ The configuration files are not encrypted by default. A configuration file may contain sensitive information about connection strings, user IDs, passwords, database servers, and catalogs. You should protect this information against unauthorized read/write operations by using encryption techniques. For information about how to encrypt configuration files, see [Configuring Enterprise Library](test-markdown_05ae7aff-85e4-46b5-8401-c4df81a2ea00.html).

