---
Source File Name: 75-Interception.docx
AssetID: 07573c82-9f99-4474-8e16-d2b8b6ea62a8
Title: The Property Matching Rule
Order In ToC: 2\6\1\7
Output Filename: 2\6\1\7_The Property Matching Rule.markdown
---

#### Markdown Test ####
# The Property Matching Rule #
----------


> ![(../../../images/note.gif)#!155CharTopicSummary!#:
> 
The property matching rule allows you to select individual properties of the target classes based on name and combination of accessors they implement.

The property matching rule allows developers, operators, and administrators to select individual properties of the target classes based on their name, including using wildcard characters, and the combination of accessors they implement.  

# Behavior of the Property Matching Rule #
The property matching rule does the following:  
+ It uses the value of the parameters passed to it to configure the matching rule for injection.
+ It compares each **propertyName** value to the name of the target object property, taking into account any wildcard characters in the match string. 
+ It performs the name comparison on a non-caseâ€“sensitive basis if the **ignoreCase **parameter is **True**, or on a case-sensitive basis if the **ignoreCase **parameter is **False**. 
+ It compares target object property accessors with the value specified and selects only parameters that have the appropriate combination of **Get** and **Set** accessors.  
+ It returns **True** if the name and the combination of **Get** and **Set** accessors of the target object matches the values in the configuration; if the name and the combination of **Get** and **Set** accessors of the target object do not match the values in the configuration, it returns **False**. 
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2.html).  

# Creating a Property Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **PropertyMatchingRule** class.  

```csharp
PropertyMatchingRule(string propertyName)

PropertyMatchingRule(string propertyName, PropertyMatchingOption option)

PropertyMatchingRule(string propertyName, PropertyMatchingOption option, bool ignoreCase)

PropertyMatchingRule(IEnumerable<PropertyMatchingInfo> matches)
```


```vb
PropertyMatchingRule(propertyName As String)

PropertyMatchingRule(propertyName As String, option As PropertyMatchingOption)

PropertyMatchingRule(propertyName As String, option As PropertyMatchingOption, ignoreCase As Boolean)

PropertyMatchingRule(matches As IEnumerable(Of PropertyMatchingInfo))
```

The following table describes the parameters shown above.  
ParameterDescriptionpropertyNameString. This is the name of the target property. It can include or consist of the * or ? wildcard characters so you can select multiple properties. You can also use square brackets [ ] to specify a range of characters. The following are examples:Get**Orders*TransactedOrder???BusinessRules[order]Process.**matchesPropertyMatchingInfo collection. The PropertyMatchingInfo class defines three items of information about each parameter that it will match:The property name as a String. A value from the PropertyMatchingOption enumeration that indicates if the rule should match on the Get, Set, or both for a selected parameter. Valid values are Get, Set, and GetOrSet.A Boolean value that specifies whether the match should be carried out on a case-sensitive basis. The default is false.ignoreCaseBoolean. This specifies whether the match should be carried out on a case-sensitive basis. The default is false.
The following code extract shows how you can add a property matching rule to a policy using the Unity interception mechanism.  

```csharp
myContainer.Configure<Interception>()
           .AddPolicy("MyPolicy")
           .AddMatchingRule<PropertyMatchingRule>
                (new InjectionConstructor
                    ("MyPropertyName", PropertyMatchingOption.GetOrSet,true))
           .AddCallHandler<MyCallHandler>
                ("MyValidator", 
                new ContainerControlledLifetimeManager());
```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
           .AddMatchingRule(Of PropertyMatchingRule) _
               (New InjectionConstructor _
                   ("MyPropertyName", PropertyMatchingOption.GetOrSet, True)) _
           .AddCallHandler(Of MyCallHandler) _
                ("MyValidator", New ContainerControlledLifetimeManager())
```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](test-markdown_2090aa6d-38c7-4527-a211-aa4fa966e855.html).  


