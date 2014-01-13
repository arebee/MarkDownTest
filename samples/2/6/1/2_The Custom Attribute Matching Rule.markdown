---
Source File Name: 75-Interception.docx
AssetID: e3c1e47a-2b21-4bff-8a1a-34f9191e4e65
Title: The Custom Attribute Matching Rule
Order In ToC: 2\6\1\2
Output Filename: 2\6\1\2_The Custom Attribute Matching Rule.markdown
---

#### Markdown Test ####
# The Custom Attribute Matching Rule #
----------


> ![(../../../images/note.gif)#!155CharTopicSummary!#:
> 
The custom attribute matching rule allows you to select target classes based on a custom attribute type that is applied to class members. 

The custom attribute matching rule allows developers, operators, and administrators to select target classes based on a custom attribute type that is applied to class members.  

# Behavior of the Custom Attribute Matching Rule #
The custom attribute matching rule does the following:  
+ It uses the value of the parameters passed to it to configure the matching rule for injection.
+ It compares the **attributeType** value to the type of any attributes that are applied to members of the target object.
+ It searches for matching attribute types in all base classes that the target class inherits from if the **inherited **parameter is **True**.
+ It returns **True** if the attribute type matches the value of the **attributeType** parameter; if the attribute type does not match the value of the **attributeType**, it returns **False**. 
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2.html).  

# Creating a Custom Attribute Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **CustomAttributeMatchingRule** class.  

```csharp
CustomAttributeMatchingRule(Type attributeType, bool inherited)
```


```vb
CustomAttributeMatchingRule(attributeType As Type, inherited As Boolean)
```

The following table describes the parameters shown above.  
ParameterDescriptionattributeTypeType. This is the type name of the custom attribute that is applied to members of the target object, such as MyCustomAttribute.inheritedBoolean. This specifies whether the rule should also search base classes for members that carry the custom attribute.
The following code extract shows how you can add a custom attribute matching rule to a policy using the Unity interception mechanism.  

```csharp
myContainer.Configure<Interception>()
           .AddPolicy("MyPolicy")
           .AddMatchingRule<CustomAttributeMatchingRule>
               (new InjectionConstructor(typeof(MyAttributeType), true))
           .AddCallHandler<MyCallHandler>
                ("MyValidator", 
                new ContainerControlledLifetimeManager());
```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
           .AddMatchingRule(Of CustomAttributeMatchingRule) _
                (New InjectionConstructor(GetAttribue(MyAttributeType), True)) _
           .AddCallHandler(Of MyCallHandler) _
               ("MyValidator", New ContainerControlledLifetimeManager())

```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](test-markdown_2090aa6d-38c7-4527-a211-aa4fa966e855.html).  


