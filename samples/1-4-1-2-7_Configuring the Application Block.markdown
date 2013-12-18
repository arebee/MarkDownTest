---
Source File Name: 40-Logging.docx
AssetID: 2b5c86d6-f126-4425-9bce-731bb2fcd52d
Title: Configuring the Application Block
Order In ToC: 1-4-1-2-7
Output Filename: 1-4-1-2-7_Configuring the Application Block.markdown
---

#### Markdown Test ####
# Configuring the Application Block #
----------

The following procedure describes how to configure the Logging Application Block properties.  
<a name="config_appblock" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>**To configure the Logging Application Block**

1. In the **Logging Settings** section, click each property you want to change. For information about the Logging Application Block properties, see the table that follows this procedure.
2. Set the properties if you need to. If you want to use a default category, click the drop-down arrow and select one of the category names. Log entries that are not assigned to a category belong to the default category.
3. The **Warn If No Category Match** property sends log entries that are assigned to a category that is not specified in configuration to the **Logging Errors &amp; Warnings** special source. The default is **True**. If you do not want this to occur, click **False** in the drop-down list.
4. The **Activity Tracing Enabled** property specifies whether activity tracing is enabled. The default is **True**. If you do not want this to occur, click **False** in the drop-down list.

# Logging Application Block Properties #
<a name="props_appblock" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following table lists the properties that appear in the **Logging Settings** section of the Logging Application Block configuration.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p><b>Attribute</b></p></th><th><p><b>Description</b></p></th></tr><tr><td><p><b>Activity Tracing Enabled</b></p></td><td><p>Specifies whether activity tracing is enabled. Possible values are <b>True</b> or <b>False</b>. The default is <b>True</b>. </p></td></tr><tr><td><p><b>Default Logging Category</b></p></td><td><p>Log entries go to this category if no other category is specified. The default is <b>General</b>. This is required.</p></td></tr><tr><td><p><b>Protection Provider</b></p></td><td><p>The protection provider that should be used to encrypt the log file entries.</p></td></tr><tr><td><p><b>Require Permission</b></p></td><td><p>Indicates whether to run your application in partial trust mode. Possible values are <b>True</b> or <b>False</b>. The default is <b>True</b>, which allows the application to run in partial trust modes.</p></td></tr><tr><td><p><b>Revert Impersonation</b></p></td><td><p>Specifies whether to opt out of impersonation-reverting. To opt out of the impersonation-reverting default mode, set <b>RevertImpersonation</b> to <b>False</b>. Support includes configuration support, and design-time support. The default is <b>True</b>. This attribute is optional.</p></td></tr><tr><td><p><b>Warn If No Category Match</b></p></td><td><p>Specifies whether events should be sent to the errors special source trace listener if a log entry contains a category that is not specified in configuration. Possible values are <b>True</b> and <b>False</b>. The default is <b>True</b>. This is optional.</p></td></tr></table><a name="_Toc253064981" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Remarks #
Performance testing has demonstrated a 5% performance overhead when reverting impersonation when logging using the rolling flat file trace listener.  

![](images/note.gif)Note:&gt; For an ASP.NET site that uses the Logging Application Block, when **Revert Impersonation** is set to **True**, the credentials used to send an e-mail will be those of the ASP.NET worker process.
