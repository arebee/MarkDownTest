---
Source File Name: 75-Interception.docx
AssetID: e7d4bacf-a864-4a50-b7c3-88acec5c4d7d
Title: The Logging Handler
Order In ToC: 2\6\3\3
Output Filename: 2\6\3\3_The Logging Handler.markdown
---

#### Markdown Test ####
# The Logging Handler #
----------


&gt; ![](/images/note.gif)#!155CharTopicSummary!#:
&gt; 
This handler uses the Logging Application Block, taking advantage of the wide range of log types, formatting, and tracing features that it provides. 
The logging handler provides the capability to write log messages and trace messages as the client code invokes the selected method or accesses the selected property of the target object. This handler uses the Logging Application Block, taking advantage of the wide range of log types, formatting, and tracing features that it provides.   
The logging handler applies both before and after the invocation of the selected method or accessing the selected property of the target object, depending on settings in the application configuration.  


&gt; ![](/images/note.gif)Note:
&gt; This call handler is implemented in Microsoft.Practices.EnterpriseLibrary.Logging.PolicyInjection namespace of the Logging Application Block in the assembly Microsoft.Practices.EnterpriseLibrary.Logging.dll.

The logging handler will initialize the Logging Application Block using the same configuration source as used to create the logging handler. By default, this will be the default configuration source. It is possible to specify an alternative configuration source if you instantiate the logging handler yourself using code. If you do this, you should create the configuration source once and use the same instance each time you create a logging handler to prevent performance issues and memory leaks.  


&gt; ![](/images/note.gif)Note:
&gt; The Enterprise Library 5.0 Configuration tool does not support **Environmental Overrides **for the logging handler **Categories**. This means you will not be able to use the configuration tools at design time to customize the run-time settings of your logging handler **Categories** configuration to suit a particular environment such as a test or instrumentation environment.

# Behavior of the Logging Handler #
The logging handler does the following:  
+ It uses the values of a list of properties that define the logging behavior.
+ It builds up an instance of the **TraceLogEntry** class, which inherits from **LogEntry**, and contains (in addition to the properties exposed by the **LogEntry** class) the following strongly-typed properties: + **TypeName **
+ **MethodName**
+ **ReturnValue**
+ **CallStack **
+ **Exception **
+ **CallTime **

&gt; ![](/images/note.gif)Note:
&gt; The values of parameters passed to the target member are available through the **ExtendedProperties** property of the **LogEntry** base class, which returns a populated **Dictionary** instance. You can optimize performance by configuring the properties the handler will collect information for and populate. Be aware that, because the properties in the preceding list are not available in the standard **LogEntry** class in the Logging Application Block, the default formatter templates will not display their values. You can use the **{property}** formatter token in the Log Formatter template to display these values—for example, **{property(TypeName)}**. 

+ It sends the **TraceLogEntry** to the Logging Application Block before, after, or both before and after the method call, depending on the configuration settings. 
+ If the configuration specifies logging after the method call and includes the option to log the call duration, the logging handler records the difference between the times when its preprocessing tasks and post-processing tasks occur, and it logs this along with any other configured values.

# Creating Instances of the Logging Handler #
When you use this call handler with the Unity interception mechanism, you must provide values for any mandatory parameters of its constructors, and optionally provide values for other parameters. If you configure the container with Enterprise Library's classes, then you want that configured **LogWriter** to be injected. If you are using an unconfigured container then you will need to get the facade's writer, which will belong to Enterprise Library's default container. If you want to inject the log writer from the container being configured, then you must provide it by using **typeof(LogWriter)** or **new ResolvedParameter&lt;LogWriter&gt;()** as the first element in the **InjectionConstructor** to specify that the constructor parameter is to be resolved The values provided are used to set the properties of the handler at run time. The constructors you can use are shown in the following code.  

```csharp
// Uses the default values. Gets the logWriter from the Logger.Writer facade method.
LogCallHandler()

LogCallHandler(LogWriter writer)

LogCallHandler(LogWriter writer, int eventId,
               bool logBeforeCall, bool logAfterCall, 
               string beforeMessage, string afterMessage,
               bool includeParameters, bool includeCallStack,
               bool includeCallTime, int priority)

LogCallHandler(LogWriter writer, int eventId,
               bool logBeforeCall, bool logAfterCall, 
               string beforeMessage, string afterMessage,
               bool includeParameters, bool includeCallStack,
               bool includeCallTime, int priority, int order)

```


```vb
LogCallHandler()

LogCallHandler(writer As LogWriter)

LogCallHandler(writer As LogWriter, eventId As Integer, _
               logBeforeCall As Boolean, logAfterCall As Boolean, _
               beforeMessage As String, afterMessage As String, _
               includeParameters As Boolean, includeCallStack As Boolean, _
               includeCallTime As Boolean, priority As Integer)

LogCallHandler(writer As LogWriter, eventId As Integer, _
               logBeforeCall As Boolean, logAfterCall As Boolean, _
               beforeMessage As String, afterMessage As String, _
               includeParameters As Boolean, includeCallStack As Boolean, _
               includeCallTime As Boolean, priority As Integer, order As Integer)
```

The following table describes the values for the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>writer</b></p></td><td><p><b>LogWriter </b>collection. The list of categories to which the logging handler will write events. Each category can be a literal value, and/or include the tokens {method}, {type}, {namespace}, and {assembly}.</p></td></tr><tr><td><p><b>eventId</b></p></td><td><p><b>Integer</b>. The ID of the event to log. </p></td></tr><tr><td><p><b>logBeforeCall</b></p></td><td><p><b>Boolean</b>. Whether to write a log entry before the call passes to the target object. Note that when this property is set to <b>true</b> in a <b>LogCallHandler</b> attribute, the <b>LogAfterCall</b> property is always also <b>true</b>.</p></td></tr><tr><td><p><b>logAfterCall</b></p></td><td><p><b>Boolean</b>. Whether to write a log entry after the call returns from the target object. Note that this property is <b>true</b> when the <b>LogBeforeCall</b> property is set to <b>true</b> in a <b>LogCallHandler</b> attribute.</p></td></tr><tr><td><p><b>beforeMessage</b></p></td><td><p><b>String</b>. The message that the Logging Handler will log before the target method executes.</p></td></tr><tr><td><p><b>afterMessage</b></p></td><td><p><b>String</b>. The message that the Logging Handler will log after the target method executes.</p></td></tr><tr><td><p><b>includeParameters</b></p></td><td><p><b>Boolean</b>. Whether to include the parameter values in the log message. </p></td></tr><tr><td><p><b>includeCallStack</b></p></td><td><p><b>Boolean</b>. Whether to include the call stack in the log message. </p></td></tr><tr><td><p><b>includeCallTime</b></p></td><td><p><b>Boolean</b>. Whether to include the call duration in the <b>AfterMessage</b> for the log entry. </p></td></tr><tr><td><p><b>priority</b></p></td><td><p><b>Integer</b>. The priority value to include in the log message.</p></td></tr><tr><td><p><b>order</b></p></td><td><p><b>Integer</b>. The position of the handler within the policy handler chain, starting from <b>1</b>. The default value is zero, which means that there is no explicit order specified for the handler in relation to other handlers in the same handler chain.</p></td></tr></table>
The Logging Handler also exposes these values as the **LogWriter**, **EventId**, **LogBeforeCall**, **LogAfterCall**, **BeforeMessage**, **AfterMessage**, **IncludeParameters**, **IncludeCallStack**, **IncludeCallTime**, **Priority**, and **Order** properties.  It also exposes the **Categories** property as a **List** of **String** values, and the **Severity** property as a value from the **TraceEventType** enumeration (such as **Critical**, **Error**, or **Warning**).  
The following code extract shows how you can add a Logging Handler to a policy using the Unity type resolution and interception mechanisms. This example specifies that **LogWriter** is to be resolved by the container and it provides values for the **LogCallHandler** property values.  

```csharp
myContainer.Configure&lt;Interception&gt;()
           .AddPolicy("MyPolicy")
               .AddMatchingRule&lt;TypeMatchingRule&gt;(
                    new InjectionConstructor("My.Order.Object", true))
               .AddCallHandler&lt;LogCallHandler&gt;( 
                  new ContainerControlledLifetimeManager(),
                  new InjectionConstructor(
                      typeof(LogWriter), 
                      9000, true, true, 
                      "started", "finished", true, false, true, 1, 5
                      ));
```


```vb
myContainer.Configure(Of Interception)() _
    .AddPolicy("MyPolicy") _
        .AddMatchingRule(Of TypeMatchingRule) _
            (New InjectionConstructor("My.Order.Object", True)) _
        .AddCallHandler(Of LogCallHandler) _
            (New ContainerControlledLifetimeManager(), _
                New InjectionConstructor(GetType(LogWriter), _
                9000, True, True, "started", "finished", _
                True, False, True, 1, 5))
```



# Using the Logging Handler Attribute #
Instead of configuring a call handler as part of a call hander pipeline, you force it to be applied by adding the appropriate attribute to your classes. The following code shows the use of the **LogCallHandler** attribute on a simple method. This attribute can also be applied to the class declaration, in which case it applies to all members of that class.   

```csharp
[LogCallHandler]
public void Deposit(decimal depositAmount)
{
  balance += depositAmount;
}
```


```vb
&lt;LogCallHandler&gt; _
Public Sub Deposit(depositAmount As Decimal)
  balance += depositAmount
End Sub
```

The following table describes the properties of the **LogCallHandlerAttribute **class.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>AfterMessage</b></p></td><td><p><b>String</b>. The message that the logging handler will log after the target method executes.</p></td></tr><tr><td><p><b>BeforeMessage</b></p></td><td><p><b>String</b>. The message that the logging handler will log before the target method executes.</p></td></tr><tr><td><p><b>Categories</b></p></td><td><p><b>String</b> collection. The list of categories to which the logging handler will write events. Each category can be a literal value, and/or include the tokens {method}, {type}, {namespace}, and {assembly}.</p></td></tr><tr><td><p><b>EventId</b></p></td><td><p><b>Int.</b> The event identifier to include in log entries.</p></td></tr><tr><td><p><b>IncludeCallStack</b></p></td><td><p><b>Boolean</b>. Whether to include the call stack in the log message. </p></td></tr><tr><td><p><b>IncludeCallTime</b></p></td><td><p><b>Boolean</b>. Whether to include the call duration in the <b>AfterMessage</b> for the log entry. </p></td></tr><tr><td><p><b>IncludeParameters</b></p></td><td><p><b>Boolean</b>. Whether to include the parameter values in the log message. </p></td></tr><tr><td><p><b>LogAfterCall</b></p></td><td><p><b>Boolean</b>. Whether to write a log entry after the call returns from the target object. Note that this property is <b>true</b> when the <b>LogBeforeCall</b> property is set to <b>true</b> in a <b>LogCallHandler</b> attribute.</p></td></tr><tr><td><p><b>LogBeforeCall</b></p></td><td><p><b>Boolean</b>. Whether to write a log entry before the call passes to the target object. Note that, when this property is set to <b>true</b> in a <b>LogCallHandler</b> attribute, the <b>LogAfterCall</b> property is always also <b>true</b>.</p></td></tr><tr><td><p><b>Order</b></p></td><td><p><b>Integer</b>. The position of the handler within the policy handler chain, starting from <b>1</b>. The default value is zero, which means that there is no explicit order specified for the handler in relation to other handlers in the same handler chain.</p></td></tr><tr><td><p><b>Priority</b></p></td><td><p><b>Integer</b>. The priority value to include in the log message.</p></td></tr><tr><td><p><b>Severity</b></p></td><td><p>The severity value of an exception to include in the log message, using values from the<b> TraceEventType</b> enumeration, such as <b>Critical</b>, <b>Error</b>, and <b>Warning</b>. </p></td></tr></table>
To set these properties using an attribute, add them as parameters to the attribute declaration, as shown in the following code.  

```csharp
[LogCallHandler(Categories=new string[]{"My Category"}, LogBeforeCall=true, 
         BeforeMessage="This occurs before the call to the target object")]
```


```vb
&lt;LogCallHandler(Categories:=New String(){"My Category"}, LogBeforeCall:=True, _
         BeforeMessage:="This occurs before the call to the target object")&gt;
```

For more information about using call handler attributes and attribute driven policies, see [Attribute-Driven Policies](test-markdown_456aac54-4ba3-4904-adae-36fb5227fabc.html).  

