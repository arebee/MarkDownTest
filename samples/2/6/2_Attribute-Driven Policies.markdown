---
Source File Name: 75-Interception.docx
AssetID: 456aac54-4ba3-4904-adae-36fb5227fabc
Title: Attribute-Driven Policies
Order In ToC: 2\6\2
Output Filename: 2\6\2_Attribute-Driven Policies.markdown
---

#### Markdown Test ####
# Attribute-Driven Policies #
----------


> ![(../../images/note.gif)#!155CharTopicSummary!#:
> 
Developers specify handlers for classes and their members (methods and properties) using call handler attributes.

A common scenario when using any policy injection framework is the requirement to specify policies for classes and their members using attributes directly applied to the appropriate classes and members. Unity interception supports this techniqueâ€”it actively discovers classes and members with attributes that define call handlers and applies the appropriate policies.  
Developers specify handlers for classes and their members (methods and properties) using call handler attributes. Each attribute automatically instantiates the appropriate call handler, and applies the values of the attribute parameters to the properties of the call handler. Using directly applied attributes has the following advantages:  
+ Developers can ensure that Unity adds handlers that are specifically required in all circumstances and which should never be removed from the handler pipeline.
+ Developers can fix the settings or values of specific parameters on classes and class membersâ€”for example, by defining that specific parameter values must always be greater than zero or that logging will always occur for specific methods.
+ Developers can prevent the application of a handler pipeline to specific methods and properties, or to whole classes, using the **ApplyNoPoliciesAttribute **attribute.
However, applying policies through attributes applied directly to members of the target classes means that developers, administrators, and operators can no longer control the behavior of interception without changing the source code and recompiling the solution. In addition, using the **ApplyNoPoliciesAttribute **attribute may cause unexpected behavior for developers, administrators, and operators, who may attempt to add policies to an application without being aware of the applied attributes.  

# Policy and Handler Precedence with Attributes #
Unity combines all the policies it discovers for each class and class member. When classes and members use directly applied attributes to define the handlers for a class or class member, and the application configuration contains matching rules that select the same class or class member, Unity applies a set of precedence rules to determine the overall combined policy to apply. The order of precedence for the discovery and application of handlers is the following:  
1. If the class or type, or any class or type in its inheritance hierarchy, has the **ApplyNoPoliciesAttribute** attribute, the discovery process ceases and Unity does not apply any policies to that class or type.
2. If the member (property or method) of the class, or in a class within its inheritance hierarchy, has the **ApplyNoPoliciesAttribute** attribute, the discovery process ceases and Unity does not apply any policies to that member.
3. If the class or type, or any class or type in its inheritance hierarchy, has any handler attributes, Unity applies these handlers and continues the discovery process. The final order of individual handlers is unspecified if you do not provide a value for the **Order** property of each one.
4. If the member (property or method) of the class (or the same member in a class within its inheritance hierarchy) carries a handler attribute, Unity applies this handler and continues the discovery process. The final order of individual handlers is unspecified if you do not provide a value for the **Order** property of each one, although handlers for property accessors (**Get** and **Set**) usually occur earlier in the handler pipeline.
5. If the configuration contains matching rules that select the class or member, Unity applies these handlers. However, it will not overwrite, change, or replace any handlers defined at higher precedence. It applies policies defined later in the configuration closer to the target (later in the handler pipeline) and applies handlers within each policy in the order in which they occur in the configuration or the order specified by for the **Order** property of each one. 

# Example of an Attribute-Driven Policy #
Unity does not contain any call handlers, and so the following example demonstrates how you can create an attribute-driven policy using the **ValidationCallHandler** that is provided with the patterns &amp; practices Enterprise Library. It shows a method named **Deposit** that has the **ValidationCallHandler** attribute applied. Unity creates a handler pipeline that contains the **ValidationCallHandler**, which itself uses the features of the Enterprise Library Validation Application Block. The Validation Application Block then uses the settings you specify with attributes on the class members, or in a rule set defined within the Validation Application Block configuration, to validate the value of the **depositAmount** parameter and ensure that the value is greater than zero.   

```csharp
[ValidationCallHandler]
public void Deposit([RangeValidator(typeof(Decimal), "0.0", 
                     RangeBoundaryType.Exclusive, "0.0", 
                     RangeBoundaryType.Ignore)] decimal depositAmount)
{
  balance += depositAmount;
}
```


```vb
&lt;ValidationCallHandler> _
Public Sub Deposit(&lt;RangeValidator(GetType(Decimal), "0.0", _
                    RangeBoundaryType.Exclusive, "0.0", _
                    RangeBoundaryType.Ignore)> Decimal depositAmount)
  balance += depositAmount
End Sub
```

For information about creating call handlers and call handler attributes, see [Creating Interception Policy Injection Call Handlers](test-markdown_587afe3d-d6c7-447f-bb9b-8fd750174bcc.html) and [Creating Interception Handler Attributes](test-markdown_f9822e7e-003d-482d-9d72-5f795704367a.html). For information about Enterprise Library call handlers, see [Enterprise Library Call Handlers](test-markdown_969b6f02-4da3-41d1-8527-c9e0009d1632.html).  


