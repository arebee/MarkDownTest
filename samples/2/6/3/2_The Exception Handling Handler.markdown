﻿---
Source File Name: 75-Interception.docx
AssetID: d874dee7-1158-4cd7-900a-d592b5da5e69
Title: The Exception Handling Handler
Order In ToC: 2\6\3\2
Output Filename: 2\6\3\2_The Exception Handling Handler.markdown
---

#### Markdown Test ####
# The Exception Handling Handler #
----------


> ![(../../../images/note.gif)#!155CharTopicSummary!#:
> 
This handler uses the Exception Handling Application Block, taking advantage of the wide range of options that it supports.

The exception handling handler provides the capability to manage and process exceptions in a standard way. This handler uses the Exception Handling Application Block, taking advantage of the wide range of options that it supports.   
The exception handler applies **after** invocation of the selected method or access to the selected property of the target object. If the method or property accessor raises an exception, the exception handling handler will invoke a named exception handling policy defined within the Exception Handling Application Block. This policy may ignore the exception, return the original exception, or replace it with a new exception. The exception handling handler then packages the exception (if the Exception Handling Application Block returns one) into the message passed back to the previous handler in the chain.  
Each instance of the exception handling handler maintains its own hierarchy of exception policies and any dependent objects. When using the logging handler with the Exception Handling Application Block, each exception handling handler instance will contain its own **LogWriter** instance and set of **TraceListeners**. If the Logging Application Block is configured to use a flat file trace listener or a rolling flat file trace listener, you may see multiple log files with GUIDs in their file names because multiple instances of the trace listeners are not able to write to the configured log file at the same time.  


> ![(../../../images/note.gif)Note:
> <a name="_Toc253065315" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>This call handler is implemented in the Microsoft.Practices.EnterpriseLibrary.ExceptionHandling.PolicyInjection namespace of the Exception Handling Application Block in the Microsoft.Practices.EnterpriseLibrary.ExceptionHandling.dll assembly.


# Behavior of the Exception Handling Handler #
The exception handling handler does the following:  
+ It accepts the name of the exception policy to use, which must match an exception policy configured in the Exception Handling Application Block.
+ It does nothing if the target method or property accessor does not throw an exception. 
+ If the target method or property accessor throws an exception, it does the following:+ It passes the exception to the Exception Handling Application Block for processing in accordance with the specified exception policy. 
+ It wraps the exception returned by the Exception Handling Application Block in a message and returns it to the previous handler, which may act upon it. 

+ If the Exception Handling Application Block does not return an exception, it does the following:+ If the target method does not return a result, it passes a null message back to the previous handler in the handler pipeline. 
+ If the target method returns a result, it generates an **InvalidOperationException,** wraps it in a message, and returns it to the previous handler. 


# Creating Instances of the Exception Handling Handler #
When you use this call handler with the Unity interception mechanism, you must provide values for any mandatory parameters of its constructors, and optionally provide values for other parameters. These values are used to set the properties of the handler at run time. The constructors you can use are shown in the following code.  

```csharp
ExceptionCallHandler(ExceptionPolicyImpl exceptionPolicy)

ExceptionCallHandler(ExceptionPolicyImpl exceptionPolicy, int order)
```


```vb
ExceptionCallHandler(exceptionPolicy As ExceptionPolicyImpl)

ExceptionCallHandler(exceptionPolicy As ExceptionPolicyImpl, order As Integer)
```

The following table describes the values for the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>exceptionPolicy</b></p></td><td><p><b>ExceptionPolicyImpl</b>. The exception handling policy to use, as configured in the Exception Handling Application Block.</p></td></tr><tr><td><p><b>order</b></p></td><td><p><b>Integer</b>. The position of the handler within the policy handler chain, starting from <b>1</b>. The default value is zero, which means that there is no explicit order specified for the handler in relation to other handlers in the same handler chain.</p></td></tr></table>
The exception handling handler also exposes these values as the **ExceptionPolicy** and **Order** properties.   
The following code extract shows how you can add an exception handling handler to a policy using the Unity type resolution and interception mechanisms.   

```csharp
ExceptionPolicyImpl myPolicy 
    = myContainer.Resolve&lt;ExceptionPolicyImpl>("MyExceptionPolicy"); 
myContainer.Configure&lt;Interception>()
           .AddPolicy("MyPolicy")
               .AddMatchingRule&lt;TypeMatchingRule>(new InjectionConstructor("My.Order.Object",
                        true))
               .AddCallHandler&lt;AuthorizationCallHandler>
                ("MyExceptionPolicy", new ContainerControlledLifetimeManager());
```


```vb
Dim myPolicy As ExceptionPolicyImpl _
    = myContainer.Resolve(Of ExceptionPolicyImpl)("MyExceptionPolicy") 
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
               .AddMatchingRule(Of TypeMatchingRule) _
                   (New InjectionConstructor("My.Order.Object", _
                   True)) _
               .AddCallHandler(Of AuthorizationCallHandler) _
                   ("MyExceptionPolicy", _
                       New ContainerControlledLifetimeManager())
```


# Using the Exception Handling Handler Attribute #
Instead of configuring a call handler as part of a call hander pipeline, you force it to be applied by adding the appropriate attribute to your classes. The call handler attribute for the exception handling handler requires the exception policy name as the only parameter. The following table lists the properties of the exception handling handler attribute.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>ExceptionPolicy</b></p></td><td><p><b>String</b>. The name of the exception policy to use, as configured in the Exception Handling Application Block.</p></td></tr><tr><td><p><b>Order</b></p></td><td><p><b>Integer</b>. The position of the handler within the policy handler chain, starting from <b>1</b>. The default value is zero, which means that there is no explicit order specified for the handler in relation to other handlers in the same handler chain.</p></td></tr></table>
<a name="handlerconfiglogging" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following code shows the use of the **ExceptionCallHandler** attribute on a simple method. This attribute can also be applied to the class declaration, in which case it applies to all members of that class.  

```csharp
[ExceptionCallHandler("exception-policy-name")]
public void Deposit(decimal depositAmount)
{
  balance += depositAmount;
}
```


```vb
&lt;ExceptionCallHandler("exception-policy-name")> _
Public Sub Deposit(depositAmount As Decimal)
  balance += depositAmount
End Sub
```

For more information about using call handler attributes and attribute-driven policies, see [Attribute-Driven Policies](test-markdown_456aac54-4ba3-4904-adae-36fb5227fabc.html).  


