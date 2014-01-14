---
Source File Name: 75-Interception.docx
AssetID: fc17bbda-834f-4f22-88f3-ccbba75bd917
Title: The Tag Attribute Matching Rule
Order In ToC: 2\6\1\9
Output Filename: 2\6\1\9_The Tag Attribute Matching Rule.markdown
---

#### Markdown Test ####
# The Tag Attribute Matching Rule #
----------


> ![(../../../images/note.gif)#!155CharTopicSummary!#:
> 
The tag attribute matching rule allows you to select target classes based on an attribute of type Tag applied to a class or to members within a class. 

The tag attribute matching rule allows developers, operators, and administrators to select target classes based on the name of an attribute of type **Tag** that is applied to a class, or to members (methods or properties) within a class. For example, the following code shows a class with two tagged members.  

```csharp
public class AnnotatedWithTags
{

  [Tag("MyTagName")]
  public void TaggedMethod(string parameter1)
  { 
    ... method implementation here ...
  }

  [Tag("AnotherTagName")]
  public string TaggedProperty
  {
     ... property implementation here ... 
  }
}
```


```vb
Public Class AnnotatedWithTags

  &lt;Tag("MyTagName")> _
  Public Sub TaggedMethod(parameter1 As String)
    ... method implementation here ...
  End Sub

  &lt;Tag("AnotherTagName")> _
  Public Property TaggedProperty As String
     ... property implementation here ... 
  End Property

End Class
```

The following code shows a tagged class.  

```csharp
[Tag("MyClassTagName")]
public class AnnotatedWithTagOnClass
{
  ... class implementation here ...
}
```


```vb
&lt;Tag("MyClassTagName")> _
Public Class AnnotatedWithTagOnClass
  ... class implementation here ...
End Class
```


# Behavior of the Tag Attribute Matching Rule #
The tag attribute matching rule does the following:  
+ It uses the value of the parameters passed to it to configure the matching rule for injection.
+ It compares the **tagToMatch **value to the full name of the **Tag** attribute. Wildcard characters are not supported for this rule.
+ It performs the comparison on a non-caseâ€“sensitive basis if the **ignoreCase **parameter is **True** or on a case-sensitive basis if the **ignoreCase **parameter is **False**. 
+ It returns **True** if the **Tag** attribute name matches the value of the **tagToMatch **parameter; if the **Tag** attribute does not match the value of the **tagToMatch **parameter, it returns **False**. 
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2.html).  

# Creating a Tag Attribute Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **TagAttributeMatchingRule** class.  

```csharp
TagAttributeMatchingRule(string tagToMatch)

TagAttributeMatchingRule(string tagToMatch, bool ignoreCase)
```


```vb
TagAttributeMatchingRule(tagToMatch As String)

TagAttributeMatchingRule(tagToMatch As String, ignoreCase As Boolean)
```

The following table describes the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Parameter</p></th><th><p>Description</p></th></tr><tr><td><p><b>tagToMatch</b></p></td><td><p><b>String</b>. This is the name of the Tag attribute that is attached to the target object, such as MyTagName. It cannot include wildcard characters.</p></td></tr><tr><td><p><b>ignoreCase</b></p></td><td><p><b>Boolean</b>. This specifies whether the match should be carried out on a case-sensitive basis. The default is false.</p></td></tr></table>
The following code extract shows how you can add a tag attribute matching rule to a policy using the Unity interception mechanism.  

```csharp
myContainer.Configure&lt;Interception>()
           .AddPolicy("MyPolicy")
           .AddMatchingRule&lt;TagAttributeMatchingRule>
               (new InjectionConstructor("MyTagName", true))
           .AddCallHandler&lt;MyCallHandler>
           ("MyValidator", 
                new ContainerControlledLifetimeManager());
```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
           .AddMatchingRule(Of TagAttributeMatchingRule) _
                (New InjectionConstructor("MyTagName", True)) _
           .AddCallHandler(Of MyCallHandler) _
                ("MyValidator", New ContainerControlledLifetimeManager())
```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](test-markdown_2090aa6d-38c7-4527-a211-aa4fa966e855.html).  


