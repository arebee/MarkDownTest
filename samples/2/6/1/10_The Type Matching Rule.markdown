---
Source File Name: 75-Interception.docx
AssetID: f745650b-dd5f-4703-be36-7b3ece55cb19
Title: The Type Matching Rule
Order In ToC: 2\6\1\10
Output Filename: 2\6\1\10_The Type Matching Rule.markdown
---

#### Markdown Test ####
# The Type Matching Rule #
----------


> ![(../../../images/note.gif)#!155CharTopicSummary!#:
> 
The type matching rule allows you to specify the target class using the namespace and class name of the target type.

The type matching rule allows developers, operators, and administrators to specify the target class using the namespace and class name of the target type.  

# Behavior of the Type Matching Rule #
The type matching rule does the following:  
+ It uses the value of the parameters passed to it to configure the matching rule for injection.
+ It compares each type or type name value specified to the type or the full namespace and class name of the target object type, or to just the class name if no namespace is provided.
+ It performs the comparison on a non-case-sensitive basis if the **ignoreCase **property is **True** or on a case-sensitive basis if the **ignoreCase **property is **False**. 
+ It returns **True** if any of the types or type name values match the target object type or namespace and class name; if none match, it returns **False**. 
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2.html).  

# Creating a Type Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **TypeMatchingRule** class.  

```csharp
TypeMatchingRule(Type type)

TypeMatchingRule(string typeName)

TypeMatchingRule(string typeName, bool ignoreCase)

TypeMatchingRule(IEnumerable<MatchingInfo> matches)
```


```vb
TypeMatchingRule(type As Type)

TypeMatchingRule(typeName As String)

TypeMatchingRule(typeName As String, ignoreCase As Boolean)

TypeMatchingRule(matches As IEnumerable(Of MatchingInfo))
```

The following table describes the parameters shown above.  
ParameterDescriptiontypeType. The type to match.typeNameString. Type name to match. This is the full namespace and class name of the target object such as MyNamespace.BusinessObjects.Orders, or just the class name such as MyOrderObject.matchesMatchingInfo collection. A list of one or more type names, using the same rules as for the typeName parameter. MatchingInfo is a class used for storing information about a single name and case sensitivity value pair.ignoreCaseBoolean. This specifies whether the match should be carried out on a case-sensitive basis. The default is false.
The following code extract shows how you can add a type matching rule to a policy using the Unity interception mechanism.  

```csharp
myContainer.Configure<Interception>()
           .AddPolicy("MyPolicy")
           .AddMatchingRule<TypeMatchingRule>
                (new InjectionConstructor("My.Order.Object", true))
           .AddCallHandler<MyCallHandler>
                ("MyValidator", 
                new ContainerControlledLifetimeManager());
```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
           .AddMatchingRule(Of TypeMatchingRule) _
                (New InjectionConstructor("My.Order.Object", True)) _
           .AddCallHandler(Of MyCallHandler) _
                ("MyValidator", New ContainerControlledLifetimeManager())
```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](test-markdown_2090aa6d-38c7-4527-a211-aa4fa966e855.html).  


