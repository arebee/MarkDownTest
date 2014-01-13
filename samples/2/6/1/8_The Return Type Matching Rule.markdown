---
Source File Name: 75-Interception.docx
AssetID: 4b09e824-1e73-4230-988f-9a8ed8f5968a
Title: The Return Type Matching Rule
Order In ToC: 2\6\1\8
Output Filename: 2\6\1\8_The Return Type Matching Rule.markdown
---

#### Markdown Test ####
# The Return Type Matching Rule #
----------


> ![(../../../images/note.gif)#!155CharTopicSummary!#:
> 
The return type matching rule allows you to select target classes based on the type or type name of the return value, using wildcards characters as needed.

The return type matching rule allows developers, operators, and administrators to select target classes based on the type or the type name of the return value, using wildcard characters if required.  

# Behavior of the Return Type Matching Rule #
The return type matching rule does the following:  
+ It uses the value of the parameters passed to it to configure the matching rule for injection.
+ It compares the return type value to the type returned by members of the target object.
+ It performs the comparison on a non-case-sensitive basis if the **ignoreCase** parameter is **True** or on a case-sensitive basis if the **ignoreCase** parameter is **False**. 
+ It returns **True** if the type or type name matches the value of the match string; if the type name does not match the value of the match string, it returns **False**. 
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2.html).  

# Creating a Return Type Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **ReturnTypeMatchingRule** class.  

```csharp
ReturnTypeMatchingRule(Type returnType)

ReturnTypeMatchingRule(string returnTypeName)

ReturnTypeMatchingRule(string returnTypeName, bool ignoreCase)
```


```vb
ReturnTypeMatchingRule(returnType As Type)

ReturnTypeMatchingRule(returnTypeName As String)

ReturnTypeMatchingRule(returnTypeName As String, ignoreCase As Boolean)
```

The following table describes the parameters shown above.  
ParameterDescriptionreturnTypeType. The type of the return value of the method to match.returnTypeNameString. The full namespace-qualified name or just the type name of the type of the return value of the method to match. The following are examples:System.Int32StringInt32ignoreCaseBoolean. This specifies whether the match should be carried out on a case-sensitive basis. The default is false.
The following code extract shows how you can add a return type matching rule to a policy using the Unity interception mechanism.  

```csharp
myContainer.Configure<Interception>()
           .AddPolicy("MyPolicy")
           .AddMatchingRule<ReturnTypeMatchingRule>
                (new InjectionConstructor("MyReturnType", true))
           .AddCallHandler<MyCallHandler>
            ("MyValidator", 
                new ContainerControlledLifetimeManager());
```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
           .AddMatchingRule(Of ReturnTypeMatchingRule) _
                (New InjectionConstructor("MyReturnType", True)) _
           .AddCallHandler(Of MyCallHandler) _
                ("MyValidator", New ContainerControlledLifetimeManager())
```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](test-markdown_2090aa6d-38c7-4527-a211-aa4fa966e855.html).  


