---
Source File Name: 75-Interception.docx
AssetID: ebe602cf-d251-4bec-ad5c-d41bbef7550b
Title: The Method Signature Matching Rule
Order In ToC: 2\6\1\4
Output Filename: 2\6\1\4_The Method Signature Matching Rule.markdown
---

#### Markdown Test ####
# The Method Signature Matching Rule #
----------


> ![](../../../images/note.gif)#!155CharTopicSummary!#:
> 
The method signature matching rule allows you to select target classes based on the name and signature (the list of parameter types) of its members.

The method signature matching rule allows developers, operators, and administrators to select target classes based on the name and signature (the list of parameter types) of its members. This rule allows the use of wildcard characters for the member names.  

# Behavior of the Method Signature Matching Rule #
The method signature matching rule does the following:  
+ It uses the value of the parameters passed to it to configure the matching rule for injection. 
+ It compares the string value of the **methodName **value with the name of the method, taking into account any wildcard characters in the match string. If no method name is specified, it matches all methods of the target class. 
+ If the method name matches, it compares each parameter type name in the list of specified names to the names of the parameters of the target member methods in the order in which they appear in the list. 
+ It performs the comparison on a non-case-sensitive basis if the **ignoreCase **value is **True** or on a case-sensitive basis if the **ignoreCase **value is **False**. 
+ It returns **True** if the method name and all the configured parameter type names match those of a target member; if the method name and all the configured parameter type names do not match those of a target member, it returns **False**. 
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](http://msdn.microsoft.com/library/af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2).  

# Creating a Method Signature Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **MethodSignatureMatchingRule** class.  

```csharp
MethodSignatureMatchingRule(IEnumerable<string> parameterTypeNames)
 
MethodSignatureMatchingRule(IEnumerable<string> parameterTypeNames, bool ignoreCase)
 
MethodSignatureMatchingRule(string methodName, IEnumerable<string> parameterTypeNames)

MethodSignatureMatchingRule(string methodName,
                            IEnumerable<string> parameterTypeNames, bool ignoreCase)
```


```vb
MethodSignatureMatchingRule(parameterTypeNames As IEnumerable(Of String))

MethodSignatureMatchingRule(parameterTypeNames As IEnumerable(Of String), ignoreCase As Boolean)
 
MethodSignatureMatchingRule(methodName As String, parameterTypeNames As IEnumerable(Of String))

MethodSignatureMatchingRule(string methodName, _
                            parameterTypeNames As IEnumerable(Of String), ignoreCase As Boolean)
```

The following table describes the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Parameter</p></th><th><p>Description</p></th></tr><tr><td><p><b>methodName</b></p></td><td><p><b>String</b>. This is the name of the target member method. It can consist of or include the * or ? wildcard characters for selecting multiple methods. You can also use square brackets [ ] to specify a range of characters. If omitted, the rule will match all methods.</p></td></tr><tr><td><p><b>parameterTypeNames</b></p></td><td><p><b>String</b> collection. This is a collection of the type names of the parameter types to match.</p></td></tr><tr><td><p><b>ignoreCase</b></p></td><td><p><b>Boolean</b>. This specifies whether the match should be carried out on a case-sensitive basis. The default is false.</p></td></tr></table>
The following code extract shows how you can add a method signature matching rule to a policy using the Unity interception mechanism.  

```csharp
IEnumerable<string> paramTypes = new List<string> {"System.String", "System.Int32"};
myContainer.Configure<Interception>()
           .AddPolicy("MyPolicy")
           .AddMatchingRule<MethodSignatureMatchingRule>
                (new InjectionConstructor("MyMethodName", paramTypes, true))
           .AddCallHandler<MyCallHandler>
                ("MyValidator", 
                new ContainerControlledLifetimeManager());

```


```vb
Dim paramTypes As IEnumerable(Of String) = New List(Of String){"System.String", "System.Int32"}
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
           .AddMatchingRule(Of MethodSignatureMatchingRule) _
               (New InjectionConstructor("MyMethodName", paramTypes, True)) _
           .AddCallHandler(Of MyCallHandler) _
               ("MyValidator", New ContainerControlledLifetimeManager())
```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](http://msdn.microsoft.com/library/2090aa6d-38c7-4527-a211-aa4fa966e855).  


