---
Source File Name: 75-Interception.docx
AssetID: 7f7053e1-9db6-433c-878f-b8a41b1d2a49
Title: The Performance Counter Handler
Order In ToC: 2\6\3\4
Output Filename: 2\6\3\4_The Performance Counter Handler.markdown
---

#### Markdown Test ####
# The Performance Counter Handler #
----------


> ![(../../../images/note.gif)#!155CharTopicSummary!#:
> 
This handler uses the instrumentation features that are part of the Enterprise Library Core. 

The performance counter handler increments a specific counter each time it executes in response to invocation of the selected method or setting of the selected property. This handler uses the instrumentation features that are part of the Enterprise Library Core.   
The performance counter handler applies both before and after invocation of the selected method or access to the selected property of the target object. The handler can increment different types of counters and increment more than one counter each time (such as a single instance and a total counter).   


> ![(../../../images/note.gif)Note:
> This call handler is implemented in the Microsoft.Practices.EnterpriseLibrary.PolicyInjection.CallHandlers namespace of the Microsoft.Practices.EnterpriseLibrary.PolicyInjection.dll assembly.


# Installing and Removing Performance Counters #
To use the performance counter handler, you must first install the performance counters into the operating system. You can use the Installutil.exe utility that ships with the .NET Framework to install or uninstall the performance counters.  

```other
installutil.exe /category=<category>[;<category> ...] Microsoft.Practices.EnterpriseLibrary.PolicyInjection.CallHandlers.dll
```

Alternatively, Enterprise Library includes a utility class named **PerformanceCountersInstaller** that you can use to install the appropriate counters. The file PerformanceCountersInstaller.cs that contains this class is in the \Source\Blocks\PolicyInjection\Src\PolicyInjection\Installers subfolder of Enterprise Library. The **PerformanceCountersInstaller** class inherits from the **Installer** class in the **System.Configuration** namespace, which exposes methods to install and uninstall performance counters.  
To install the performance counters required by the performance counter handler, the client application must create a new instance of the **PerformanceCountersInstaller** class using the default configuration source. The constructor of the **PerformanceCountersInstaller** class automatically reads the performance counter categories defined in the application configuration. After that, the code can create a new hash table to hold the counter state and an instance of the **InstallContext** class to hold the parameters for the counters, which it assigns to the **Context** of the **PerformanceCountersInstaller** instance. To install the counters, the code then calls the **Install** and **Commit** methods of the **PerformanceCountersInstaller** instance, passing the hash table containing the state to each one.  

```csharp
PerformanceCountersInstaller installer = new PerformanceCountersInstaller(new SystemConfigurationSource());
IDictionary state = new System.Collections.Hashtable();
installer.Context = new InstallContext();
installer.Install(state);
installer.Commit(state);
MessageBox.Show("Performance counters have been successfully installed.",
                 this.Text, MessageBoxButtons.OK, MessageBoxIcon.Information);
```


```vb
Dim installer As New PerformanceCountersInstaller(New SystemConfigurationSource())
Dim state As New System.Collections.Hashtable()
installer.Context = New InstallContext()
installer.Install(state)
installer.Commit(state)
MessageBox.Show("Performance counters have been successfully installed.", _
                 Me.Text, MessageBoxButtons.OK, MessageBoxIcon.Information)
```

To uninstall the performance counters for the performance counter handler, the client application again creates an instance of the **PerformanceCountersInstaller** class and the **InstallContext** class, and then it calls the **Uninstall** method of the **PerformanceCountersInstaller** instance.  

```csharp
PerformanceCountersInstaller installer = new PerformanceCountersInstaller(new SystemConfigurationSource());
installer.Context = new InstallContext();
installer.Uninstall(null);
MessageBox.Show("Performance counters have been successfully uninstalled.", 
                 this.Text, MessageBoxButtons.OK, MessageBoxIcon.Information);
```


```vb
Dim installer As New PerformanceCountersInstaller(New SystemConfigurationSource())
installer.Context = New InstallContext()
installer.Uninstall(Nothing)
MessageBox.Show("Performance counters have been successfully uninstalled.", _
                 Me.Text, MessageBoxButtons.OK, MessageBoxIcon.Information)
```

The **PerformanceCountersInstaller** class overrides the **OnBeforeInstall** and **OnBeforeUninstall** methods of the **Installer** base class to generate the category for the counters exposed by the performance counter handler, and it populates the category with the counter instances for the handler.  
The **PerformanceCountersInstaller** class also exposes overloads of the constructor that accept a **String** collection containing the category names to install or uninstall and a default constructor that generates an empty list of categories.  

# Behavior of the Performance Counter Handler #
The performance counter handler does the following:  
+ It takes details of the counters to increment, including the counter type and counter method name. 
+ It increments any counters configured for inclusion in the preprocessing stage.
+ It invokes the selected target method or sets the selected target property.
+ It increments any counters configured for inclusion in the post-processing stage.
+ It is configurable to work with counters of the following types: + Total number of calls to the target method or property 
+ Average number of calls to the target method or property per second 
+ Average duration of calls to the target method or property
+ Total number of exceptions encountered during calls to the target method or property
+ Average number of exceptions encountered per second during calls to the target method or property per second
+ Percentage of exceptions to successful calls for the target method or property


# Creating Instances of the Performance Counter Handler #
When you use this call handler with the Unity interception mechanism, you must provide values for any mandatory parameters of its constructors, and optionally provide values for other parameters. These values are used to set the properties of the handler at run time. The constructors you can use are shown in the following code.  

```csharp
PerformanceCounterCallHandler(string category, string counterInstanceName)

PerformanceCounterCallHandler(string category, string instanceName,
                              bool useTotalCounter,
                              bool incrementNumberOfCalls,
                              bool incrementCallsPerSecond,
                              bool incrementAverageCallDuration,
                              bool incrementTotalExceptions,
                              bool incrementExceptionsPerSecond)

PerformanceCounterCallHandler(string category, string instanceName,
                              bool useTotalCounter,
                              bool incrementNumberOfCalls,
                              bool incrementCallsPerSecond,
                              bool incrementAverageCallDuration,
                              bool incrementTotalExceptions,
                              bool incrementExceptionsPerSecond,
                              int handlerOrder)
```


```vb
PerformanceCounterCallHandler(category As String, counterInstanceName As String)

PerformanceCounterCallHandler(category As String, instanceName As String, _
                              useTotalCounter As Boolean, _
                              incrementNumberOfCalls As Boolean, _
                              incrementCallsPerSecond As Boolean, _
                              incrementAverageCallDuration As Boolean, _
                              incrementTotalExceptions As Boolean, _
                              incrementExceptionsPerSecond As Boolean)

PerformanceCounterCallHandler(category As String, instanceName As String, _
                              useTotalCounter As Boolean, _
                              incrementNumberOfCalls As Boolean, _
                              incrementCallsPerSecond As Boolean, _
                              incrementAverageCallDuration As Boolean, _
                              incrementTotalExceptions As Boolean, _
                              incrementExceptionsPerSecond As Boolean, _
                              handlerOrder As Integer)
```

The following table describes the values for the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>category</b></p></td><td><p><b>String</b>. The name of the category of the target performance counter.</p></td></tr><tr><td><p><b>instanceName</b></p></td><td><p><b>String</b>. The name of the instance of the target performance counter. Can include the tokens {method}, {type}, {namespace}, {assembly}, and {appdomain}.</p></td></tr><tr><td><p><b>counterInstanceName</b></p></td><td><p><b>String</b>. The name of the instance of the target performance counter. Can include the tokens {method}, {type}, {namespace}, {assembly}, and {appdomain}.</p></td></tr><tr><td><p><b>useTotalCounter</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Total" counter each time. </p></td></tr><tr><td><p><b>incrementNumberOfCalls</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Total number of calls" counter each time. </p></td></tr><tr><td><p><b>incrementCallsPerSecond</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Number of calls per second" counter each time. </p></td></tr><tr><td><p><b>incrementAverageCallDuration</b></p></td><td><p><b>Boolean</b>. Whether to increment an "Average duration of each call" counter each time. </p></td></tr><tr><td><p><b>incrementTotalExceptions</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Total number of exceptions" counter each time.</p></td></tr><tr><td><p><b>incrementExceptionsPerSecond</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Number of exceptions per second" counter each time.</p></td></tr><tr><td><p><b>handlerOrder</b></p></td><td><p><b>Integer</b>. The position of the handler within the policy handler chain, starting from <b>1</b>. The default value is zero, which means that there is no explicit order specified for the handler in relation to other handlers in the same handler chain.</p></td></tr></table>
The performance counter handler also exposes these values as the **Category**, **InstanceName**, **IncrementAverageCallDuration**, **IncrementCallsPerSecond**, **IncrementExceptionsPerSecond**, **IncrementNumberOfCalls**, **IncrementTotalExceptions**, **UseTotalCounter**, and **Order** properties.  
The following code extract shows how you can add a Performance Counter Handler to a policy using the Unity interception mechanism.   

```csharp
myContainer.Configure<Interception>()
           .AddPolicy("MyPolicy")
               .AddMatchingRule<TypeMatchingRule>(new InjectionConstructor("My.Order.Object", true))
               .AddCallHandler<PerformanceCounterCallHandler>
                ("MyValidator", 
                new ContainerControlledLifetimeManager());
 

```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
                .AddMatchingRule(Of TypeMatchingRule) _
                    (New InjectionConstructor("My.Order.Object", True))
               .AddCallHandler(Of PerformanceCounterCallHandler)("MyValidator", New ContainerControlledLifetimeManager())
```


# Using the Performance Counter Handler Attribute #
Instead of configuring a call handler as part of a call hander pipeline, you force it to be applied by adding the appropriate attribute to your classes. The following code shows the use of the **PerformanceCounterCallHandler** attribute on a simple method. This attribute can also be applied to the class declaration, in which case it applies to all members of that class. The category name and instance name of the counter to update are mandatory parameters of the **PerformanceCounterCallHandler** attribute.   

```csharp
[PerformanceCounterCallHandler("category-name", "instance-name")]
public void Deposit(decimal depositAmount)
{
  balance += depositAmount;
}
```


```vb
<PerformanceCounterCallHandler("category-name", "instance-name")> _
Public Sub Deposit(depositAmount As Decimal)
  balance += depositAmount
End Sub
```

The following table describes the properties of the **PerformanceCounterCallHandlerAttribute **class.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>CategoryName</b></p></td><td><p><b>String</b>. The name of the category of the target performance counter.</p></td></tr><tr><td><p><b>InstanceName</b></p></td><td><p><b>String</b>. The name of the instance of the target performance counter. Can include the tokens {method}, {type}, {namespace}, {assembly}, and {appdomain}.</p></td></tr><tr><td><p><b>IncrementAverageCallDuration</b></p></td><td><p><b>Boolean</b>. Whether to increment an "Average duration of each call" counter each time. </p></td></tr><tr><td><p><b>IncrementCallsPerSecond</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Number of calls per second" counter each time. </p></td></tr><tr><td><p><b>IncrementExceptionsPerSecond</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Number of exceptions per second" counter each time.</p></td></tr><tr><td><p><b>IncrementNumberOfCalls</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Total number of calls" counter each time. </p></td></tr><tr><td><p><b>IncrementTotalExceptions</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Total number of exceptions" counter each time.</p></td></tr><tr><td><p><b>UseTotalCounter</b></p></td><td><p><b>Boolean</b>. Whether to increment a "Total" counter each time. </p></td></tr></table>
To set these properties using an attribute, add them as parameters to the attribute declaration, as shown in the following code.  

```csharp
[PerformanceCounterCallHandler(CategoryName="My Category", 
                               InstanceName="MyInstance", UseTotalCounter=false)]
```


```vb
<PerformanceCounterCallHandler(CategoryName:="My Category", _
                               InstanceName:="MyInstance", UseTotalCounter:=False)>
```

For more information about using call handler attributes and attribute-driven policies, see [Attribute-Driven Policies](test-markdown_456aac54-4ba3-4904-adae-36fb5227fabc.html).  


