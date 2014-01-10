---
Source File Name: 75-Interception.docx
AssetID: f5b9b0a8-66fd-4c47-b379-b49865ccc2c9
Title: The Namespace Matching Rule
Order In ToC: 2\6\1\5
Output Filename: 2\6\1\5_The Namespace Matching Rule.markdown
---

#### Markdown Test ####
# The Namespace Matching Rule #
----------


&gt; ![](images/note.gif)#!155CharTopicSummary!#:
&gt; 
The namespace matching rule allows you to select target classes based on their namespace, using wildcard characters for the child namespaces.
The namespace matching rule allows developers, operators, and administrators to select target classes based on their namespace, using wildcard characters for the child namespace names but not for the root namespace name.  

# Behavior of the Namespace Matching Rule #
The namespace matching rule does the following:  
+ It uses the value of the parameters passed to it to configure the matching rule for injection.
+ It compares each match string to the namespace of the target object, taking into account any wildcard characters for child namespaces. Note that the match string must specify the full root namespace name followed by a period (**.**). 
+ It performs the comparison on a non-case-sensitive basis if the **ignoreCase** parameter is **True**, or on a case-sensitive basis if the **ignoreCase **property is **False**. 
+ It returns **True** if the namespace of the target object matches the value of any of the namespace name values; if the namespace of the target object does not match the value of any of the namespace name values, it returns **False**. 
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2.html).  

# Creating a Namespace Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **NamespaceMatchingRule** class.  

```csharp
NamespaceMatchingRule(string namespaceName)

NamespaceMatchingRule(string namespaceName, bool ignoreCase)

NamespaceMatchingRule(IEnumerable&lt;MatchingInfo&gt; matches)
```


```vb
NamespaceMatchingRule(namespaceName As String)

NamespaceMatchingRule(namespaceName As String, ignoreCase As Boolean)

NamespaceMatchingRule(matches As IEnumerable(Of MatchingInfo))
```

The following table describes the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Parameter</p></th><th><p>Description</p></th></tr><tr><td><p><b>namespaceName</b></p></td><td><p><b>String</b>. This is the namespace of the target object, such as MyObjects.BusinessRules or System.Collections. It can include the * or ? wildcard characters for selecting multiple child namespaces. The following are examples:</p><p>System.Collections.*</p><p>MyObjects.Order*</p><p>MyObjects.Order??</p><p>MyObjects.*</p></td></tr><tr><td><p><b>matches</b></p></td><td><p><b>MatchingInfo </b>collection. A list of one or more namespaces, using the same rules as for the <b>namespaceName </b>parameter. <b>MatchingInfo</b> is a class used for storing information about a single name and case sensitivity value pair.</p></td></tr><tr><td><p><b>ignoreCase</b></p></td><td><p><b>Boolean</b>. This specifies whether the match should be carried out on a case-sensitive basis. The default is false.</p></td></tr></table>
The following code extract shows how you can add a namespace matching rule to a policy using the Unity interception mechanism.  

```csharp
myContainer.Configure&lt;Interception&gt;()
           .AddPolicy("MyPolicy")
           .AddMatchingRule&lt;NamespaceMatchingRule&gt;
                (new InjectionConstructor("My.Namespace.Name", true))
           .AddCallHandler&lt;MyCallHandler&gt;
                ("MyValidator", 
                new ContainerControlledLifetimeManager());
```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
           .AddMatchingRule(Of NamespaceMatchingRule) _
                (New InjectionConstructor("My.Namespace.Name", True)) _
           .AddCallHandler(Of MyCallHandler) _
                ("MyValidator", New ContainerControlledLifetimeManager()) 
```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](test-markdown_2090aa6d-38c7-4527-a211-aa4fa966e855.html).  

