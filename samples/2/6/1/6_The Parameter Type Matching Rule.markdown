---
Source File Name: 75-Interception.docx
AssetID: ff549bb6-e05a-4ea9-82e8-11516e1eafea
Title: The Parameter Type Matching Rule
Order In ToC: 2\6\1\6
Output Filename: 2\6\1\6_The Parameter Type Matching Rule.markdown
---

#### Markdown Test ####
# The Parameter Type Matching Rule #
----------


> ![(../../../images/note.gif)#!155CharTopicSummary!#:
> 
The parameter type matching rule allows you to select target classes based on the type name of a parameter for a member of the target object.

The parameter type matching rule allows developers, operators, and administrators to select target classes based on the type name of a parameter for a member of the target object.  

# Behavior of the Parameter Type Matching Rule #
The parameter type matching rule does the following:  
+ It uses the value of the parameters passed to it to configure the matching rule for injection.
+ It compares each match value to the fully qualified namespace and class name, or just the class name, of the parameter types of members of the target object.
+ It compares each matching parameter to the value from the **ParameterKind **property specified in the list of parameter matches. This value can be **Input**, **Output**, **InputOrOutput**, or **ReturnValue**.
+ It performs the comparison on a non-case-sensitive basis if the **ignoreCase **value specified in the list of parameter matches is **True**, or on a case-sensitive basis if it is **False**. 
+ It returns **True** if the target object member has parameters of matching types; if the target member does not have parameter of matching types, it returns **False**. 
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2.html).  

# Creating a Parameter Type Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **ParameterTypeMatchingRule** class.  

```csharp
ParameterTypeMatchingRule(IEnumerable&lt;ParameterTypeMatchingInfo> matches)
```


```vb
ParameterTypeMatchingRule(matches As IEnumerable(Of ParameterTypeMatchingInfo))
```

The following table describes the parameter shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Parameter</p></th><th><p>Description</p></th></tr><tr><td><p><b>matches</b></p></td><td><p><b>ParameterTypeMatchingInfo</b> collection.<b> </b>The <b>ParameterTypeMatchingInfo</b> class defines three items of information about each parameter that it will match:</p><p>The parameter type name as a <b>String</b>. </p><p>A <b>Boolean</b> value that specifies whether the match should be carried out on a case-sensitive basis. The default is false.</p><p>The usage of the parameter as a value from the <b>ParameterKind</b> enumeration. Valid values are <b>Input</b>, <b>Output</b>, <b>InputOrOutput</b>, and <b>ReturnValue</b>.</p></td></tr></table>
The parameter type matching rule also exposes the **ParameterMatches** property as an **IEnumerable** collection of **ParameterTypeMatchingInfo** instances.  
The following code extract shows how you can add a parameter type matching rule to a policy using the Unity interception mechanism.  

```csharp
ParameterTypeMatchingInfo paramsUsed = new ParameterTypeMatchingInfo[]
                    {
                        new ParameterTypeMatchingInfo("System.String", false, ParameterKind.Input)
                    };
myContainer.Configure&lt;Interception>()
           .AddPolicy("MyPolicy")
           .AddMatchingRule&lt;ParameterTypeMatchingRule>
                (new InjectionConstructor(paramsUsed))
           .AddCallHandler&lt;MyCallHandler>
                ("ContentValidator", 
                new ContainerControlledLifetimeManager());
```


```vb
Dim paramsUsed As ParameterTypeMatchingInfo() = New ParameterTypeMatchingInfo() _
    {New ParameterTypeMatchingInfo("System.String", False, ParameterKind.Input)}
myContainer.Configure(Of Interception)() _
    .AddPolicy("MyPolicy") _
    .AddMatchingRule(Of ParameterTypeMatchingRule)(New InjectionConstructor(paramsUsed)) _
    .AddCallHandler(Of MyCallHandler) _
        ("ContentValidator", New ContainerControlledLifetimeManager())


```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](test-markdown_2090aa6d-38c7-4527-a211-aa4fa966e855.html).  


