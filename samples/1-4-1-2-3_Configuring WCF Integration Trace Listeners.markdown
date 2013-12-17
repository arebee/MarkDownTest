---
Source File Name: 40-Logging.docx
AssetID: 4b216b82-b77d-41c3-bb14-cc1bf5d29db2
Title: Configuring WCF Integration Trace Listeners
Order In ToC: 1-4-1-2-3
Output Filename: 1-4-1-2-3_Configuring WCF Integration Trace Listeners.markdown
---

#### Markdown Test ####
# Configuring WCF Integration Trace Listeners #
----------

Windows Communication Foundation (WCF) is able to log directly only to **System.Diagnostics** trace sources, so it is necessary to configure a special trace listener named the **EntLibLoggingProxyTraceListener** in the &lt;**system.diagnostics**&gt; configuration section to enable the Logging Application Block to process WCF log messages. This trace listener receives messages from the trace source, wraps them in a **LogEntry** object, and forwards them to the Logging Application Block, where they can be processed according to the Logging Application Block’s configuration. If the original message is in XML format (as is the case when messages are generated from WCF), the **EntLibLoggingProxyTraceListener **creates an **XmlLogEntry **object instead of a **LogEntry**. The **XmlLogEntry** class is derived from the standard **LogEntry** class and adds support for an XML payload.  
The **EntLibLoggingProxyTraceListener **will add the name of its containing trace source as a category to each **XmlLogEntry **it creates. In addition, it is possible to configure the **EntLibLoggingProxyTraceListener **to extract information from the XML data for use as additional categories. This can be specified using XPath queries in the definition of the trace listener in the configuration file. The **categoriesXPathQueries **attribute can be set to a semicolon-delimited list of XPath queries, and the **namespaces** attribute can be set to a space-delimited list of XML namespaces used in the XPath queries, as shown in the following example.    

```other
&lt;add name="entlibproxywithmultiplexpaths"
     type="Microsoft.Practices.EnterpriseLibrary.Logging.TraceListeners.EntLibLoggingProxyTraceListener, 
           Microsoft.Practices.EnterpriseLibrary.Logging"
     categoriesXPathQueries="//MessageLogTraceRecord/@Source;//MessageLogTraceRecord/@Source2"
     namespaces="xmlns:pre='urn:test' xmlns:pre2='urn:test2'"/&gt;
```

To use the **EntLibLoggingProxyTraceListener **with WCF, you will need to define a trace source named **System.ServiceModel** in the &lt;**system.diagnostics**&gt; configuration section and turn on logging in WCF by specifying appropriate values in the &lt;**diagnostics**&gt; section in the &lt;**system.serviceModel**&gt; configuration section. Note that the Enterprise Library configuration tools do not support editing either of these sections, so you must use a text editor or alternative editor. For more information, see <a href="http://msdn2.microsoft.com/en-us/library/system.servicemodel.aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">System.ServiceModel Namespace</a> on MSDN. For more information about using tracing with WCF, see <a href="http://msdn2.microsoft.com/en-us/library/ms733025.aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Configuring Tracing</a> on MSDN.  
Although you can use any of the trace listeners supported by the Logging Application Block with WCF, the most common scenario is to log the messages in XML format. XML files logged from WCF can be analyzed in the Service Trace Viewer application that is included in the Windows SDK. To configure the Logging Application Block to log messages in this XML format, you should use the **XmlTraceListener**. This trace listener derives from the **XmlWriterTraceListener**, which is a part of the .NET Framework, and is able to extract the XML payload from an **XmlLogEntry** object and write this data to an XML text file. You can analyze the output of this trace listener with the WCF log file analysis tools.   
A sample configuration file that demonstrates what the configuration should look like follows the next procedure. The &lt;**system.serviceModel**&gt; section of the file defines how the WCF service behaves and is not relevant to the actual logging process.   
The following procedure describes how to integrate the Logging Application Block with applications that use WCF.  
<a name="Configure_for_WCF" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>**To configure the WCF-integration trace listeners**

1. Create or open a configuration file in one of the Enterprise Library configuration tools, and ensure the Logging Application Block is added to the application’s configuration. For more information, see <a href="test-markdown_05ae7aff-85e4-46b5-8401-c4df81a2ea00.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Configuring Enterprise Library</a>.
2. Click the plus sign icon in the **Logging** **Target Listeners **pane, point to **Add Logging Target Listener**, and then click **Add** **XML Trace Listener**.
3. (Optional) In the properties pane, set the **Name**, the **File Name** for the trace file, the **Severity Filter** for the level of message to detect, and select a value for the **Trace Output Options** property to specify which options or elements should be included in the trace output. For more information, see <a href="test-markdown_e0902ea8-47a8-465f-a7d4-6809f35f6bc8.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">TraceOutputOptions Values</a>.
4. Click the plus sign icon in the **Categories **pane and click **Add Category**.
5. In the properties pane, set the **Name **to **System.ServiceModel**; set **Auto Flush** and **Minimum Severity** as required.
6. Click the **Listeners** property plus sign icon, then select the **XML Trace Listener** from the drop-down list.
7. Sa**ve the configurati**on file.
8. Open the configuration file either in Visual Studio® or in the text editor of your choice.
9. Define the EntLib Proxy trace listener in the **&lt;system.diagnostics&gt;** section and use **System.ServiceModel** as the source. (See the sample configuration file.)
10. Modify the WCF configuration to specify the desired level of logging, as shown in the following sample configuration file.

```other
&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;configuration&gt;
  &lt;configSections&gt;
    &lt;section name="loggingConfiguration"
             type="Microsoft.Practices.EnterpriseLibrary
                  .Logging.Configuration.LoggingSettings,
                  Microsoft.Practices.EnterpriseLibrary.Logging" /&gt;
  &lt;/configSections&gt;
  &lt;loggingConfiguration name="Logging Application Block"
                        tracingEnabled="true"
                        defaultCategory="System.ServiceModel"
                        logWarningsWhenNoCategoriesMatch="true"&gt;
    &lt;listeners&gt;
      &lt;add fileName="c:\\trace-xml.log"
           listenerDataType="Microsoft.Practices.EnterpriseLibrary
                            .Logging.Configuration.XmlTraceListenerData,
                            Microsoft.Practices.EnterpriseLibrary.Logging"
           traceOutputOptions="None"
           type="Microsoft.Practices.EnterpriseLibrary.Logging
                 .TraceListeners.XmlTraceListener,
                 Microsoft.Practices.EnterpriseLibrary.Logging"
           name="XML Trace Listener" /&gt;
    &lt;/listeners&gt;
    &lt;formatters&gt;
    &lt;/formatters&gt;
    &lt;categorySources&gt;
      &lt;add switchValue="All" name="System.ServiceModel"&gt;
        &lt;listeners&gt;
          &lt;add name="XML Trace Listener" /&gt;
        &lt;/listeners&gt;
      &lt;/add&gt;
    &lt;/categorySources&gt;
    &lt;specialSources&gt;
      &lt;allEvents switchValue="All" name="All Events" /&gt;
      &lt;notProcessed switchValue="All" name="Unprocessed Category" /&gt;
      &lt;errors switchValue="All" name="Logging Errors &amp;amp; Warnings" /&gt;
    &lt;/specialSources&gt;
  &lt;/loggingConfiguration&gt;
  &lt;system.serviceModel&gt;
     &lt;diagnostics&gt;
       &lt;messageLogging logEntireMessage="true"
                       logMalformedMessages="true"
                       logMessagesAtTransportLevel="true" /&gt;
     &lt;/diagnostics&gt;
    &lt;services&gt;
    &lt;service name="WCFServiceLibrary1.service1"
             behaviorConfiguration="MyServiceTypeBehaviors" &gt;
      &lt;endpoint contract="WCFServiceLibrary1.IService1"
                binding="wsHttpBinding"/&gt;
      &lt;endpoint contract="IMetadataExchange" binding="mexHttpBinding"
                address="mex" /&gt;  
    &lt;/service&gt;
  &lt;/services&gt;
  &lt;behaviors&gt;
    &lt;serviceBehaviors&gt;
      &lt;behavior name="MyServiceTypeBehaviors" &gt;
        &lt;serviceMetadata httpGetEnabled="true" /&gt;
      &lt;/behavior&gt;
    &lt;/serviceBehaviors&gt;
  &lt;/behaviors&gt;
  &lt;/system.serviceModel&gt;
  &lt;system.diagnostics&gt;
    &lt;sources&gt;
      &lt;source name="System.ServiceModel" switchValue="All"&gt;
        &lt;listeners&gt;
          &lt;add name="traceListener" 
               type="Microsoft.Practices.EnterpriseLibrary.Logging
                     .TraceListeners.EntLibLoggingProxyTraceListener,
                     Microsoft.Practices.EnterpriseLibrary.Logging" /&gt;
        &lt;/listeners&gt;
      &lt;/source&gt;
    &lt;/sources&gt;
  &lt;/system.diagnostics&gt;
&lt;/configuration&gt;
```

For information about the WCF-integration trace listener properties, see <a href="test-markdown_b45ee518-82b1-426c-b772-1e6c0fde455e.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Trace Listener Properties</a>.  
<a name="tracelistener" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="Trace_Listener_Prop" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>
