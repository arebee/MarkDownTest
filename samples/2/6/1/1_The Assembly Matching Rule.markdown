---
Source File Name: 75-Interception.docx
AssetID: 4c47f30e-455d-4056-b773-69e6618c96fd
Title: The Assembly Matching Rule
Order In ToC: 2\6\1\1
Output Filename: 2\6\1\1_The Assembly Matching Rule.markdown
---

#### Markdown Test ####
# The Assembly Matching Rule #
----------


> ![](../../../images/note.gif)#!155CharTopicSummary!#:
> 
The assembly matching rule allows you to select target classes based on the assembly name or by specifying a reference to an assembly.

The assembly matching rule allows developers, operators, and administrators to select target classes based on the assembly name or by specifying a reference to an assembly.  

# Behavior of the Assembly Matching Rule #
The assembly matching rule does the following:  
+ It uses the value of the parameter passed to it to configure the matching rule for injection. 
+ It compares the **assemblyName** value to the name and version; the name, version and culture; or the fully qualified assembly name and details of the target assembly excluding the .dll file name extension. Alternatively, it tests if the specified **Assembly** reference is the same as the assembly it is matching against.
+ It returns **True** if the assembly matches the specified name or types; **False **if it does not.
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](http://msdn.microsoft.com/library/af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2).  

# Creating an Assembly Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **AssemblyMatchingRule** class.  

```csharp
AssemblyMatchingRule(Assembly assembly)

AssemblyMatchingRule(string assemblyName)
```


```vb
AssemblyMatchingRule(assembly As Assembly)

AssemblyMatchingRule(assemblyName As String)
```

The following table describes the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Parameter</p></th><th><p>Description</p></th></tr><tr><td><p><b>assembly</b></p></td><td><p><b>Assembly</b>. This is a reference to the assembly object to match.</p></td></tr><tr><td><p><b>assemblyName</b></p></td><td><p><b>String</b>. This is the name of the assembly to match. It can be the name and version; the name, version and culture; or the full assembly name of the assembly excluding the .dll file name extension. It cannot include wildcard characters. The following are some examples:</p><p>"Contoso.BusinessObjects.Sales"</p><p>"Contoso.BusinessObjects.Sales, Contoso.BusinessObjects"</p><p>"Contoso.BusinessObjects.Sales, Contoso.BusinessObjects,  Version=1.0.0.0"</p><p>"Contoso.BusinessObjects.Sales, Contoso.BusinessObjects,  Version=1.0.0.0, Culture=neutral"</p><p>"Contoso.BusinessObjects.Sales, Contoso.BusinessObjects,  Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"</p></td></tr></table>
The following code extract shows how you can add an assembly matching rule to a policy using the Unity interception mechanism.  

```other
myContainer.Configure<Interception>()
           .AddPolicy("MyPolicy")
           .AddMatchingRule<AssemblyMatchingRule>
                (new InjectionConstructor("my.assembly.name"))
           .AddCallHandler<MyCallHandler>
                ("NamespaceMatchHandler", 
                new ContainerControlledLifetimeManager());
```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
           .AddMatchingRule(Of AssemblyMatchingRule) _
                (New InjectionConstructor("my.assembly.name")) _
           .AddCallHandler(Of MyCallHandler) _
                ("NamespaceMatchHandler", New ContainerControlledLifetimeManager())
```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](http://msdn.microsoft.com/library/2090aa6d-38c7-4527-a211-aa4fa966e855).  


