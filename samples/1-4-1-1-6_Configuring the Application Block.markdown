---
Source File Name: 40-Logging.docx
AssetID: 1174dfab-9d42-47c2-ae57-c53b10c53986
Title: Configuring the Application Block
Order In ToC: 1-4-1-1-6
Output Filename: 1-4-1-1-6_Configuring the Application Block.markdown
---

#### Markdown Test ####
# Configuring the Application Block #
----------

The **LoggingConfiguration** class has a number of properties that enable you to configure global behavior in the Logging Application Block. The following table describes these properties.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>IsTracingEnabled</b></p></td><td><p>Specifies whether activity tracing is enabled. Possible values are <b>True</b> or <b>False</b>. The default is <b>True</b>. </p></td></tr><tr><td><p><b>DefaultSource</b></p></td><td><p>Log entries go to this category if no other category is specified. The default is <b>General</b>.</p></td></tr><tr><td><p><b>UseImpersonation</b></p></td><td><p>Specifies whether to opt out of impersonation-reverting. To opt out of the impersonation-reverting default mode, set <b>RevertImpersonation</b> to <b>False</b>. Support includes configuration support, and design-time support. The default is <b>True</b>.</p></td></tr><tr><td><p><b>LogWarningsWhenNoCategoriesMatch</b></p></td><td><p>Specifies whether events should be sent to the errors special source trace listener if a log entry contains a category that is not specified in configuration. Possible values are <b>True</b> and <b>False</b>. The default is <b>True</b>.</p></td></tr></table>

# Remarks #
Performance testing has demonstrated a 5% performance overhead when reverting impersonation when logging using the rolling flat file trace listener.  

<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>For an ASP.NET site that uses the Logging Application Block, when **Revert Impersonation** is set to **True**, the credentials used to send an e-mail will be those of the ASP.NET worker process.</td></tr></table><p /></div>