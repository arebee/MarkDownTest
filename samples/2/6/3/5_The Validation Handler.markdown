---
Source File Name: 75-Interception.docx
AssetID: ad452cb9-20c7-4db2-9801-73417714f46c
Title: The Validation Handler
Order In ToC: 2\6\3\5
Output Filename: 2\6\3\5_The Validation Handler.markdown
---

#### Markdown Test ####
# The Validation Handler #
----------


> ![(../../../images/note.gif)#!155CharTopicSummary!#:
> 
This handler uses the Validation Application Block, taking advantage of the wide range of capabilities that it offers.

The validation handler provides the capability to test whether the value provided for the selected property, or the values specified for the parameters of the selected method, are valid against specific rules. This handler uses the Validation Application Block, taking advantage of the wide range of capabilities that it offers.   
The validation handler applies the validation befor**e **invoking the method or setting the property of the target object. If validation fails, the validation handler aborts execution of the preprocessing handler pipeline, does not invoke the method or set the property, and raises an **ArgumentValidationException**.  


> ![(../../../images/note.gif)Note:
> This call handler is implemented in Microsoft.Practices.EnterpriseLibrary.Validation.PolicyInjection namespace of the Validation Application Block, in the assembly Microsoft.Practices.EnterpriseLibrary.Validation.dll.


# Behavior of the Validation Handler #
The validation handler does the following:  
+ It uses a custom rule set if provided, and the specification source (the rule location). 
+ It looks for validation attributes explicitly specifiedÂ on parameters, such as in the following example. 
```csharp
public void Deposit([RangeValidator(typeof(Decimal), "0.0",
                     RangeBoundaryType.Exclusive, "0.0", 
                     RangeBoundaryType.Ignore)] decimal depositAmount)
{
  balance += depositAmount;
}
```


```vb
Public Sub Deposit(<RangeValidator((GetType([Decimal]), "0.0", _
                     RangeBoundaryType.Exclusive, "0.0", _
                     RangeBoundaryType.Ignore)> depositAmount As Decimal)
  balance += depositAmount
End Sub
```


+ It looks for validation attached to parameter types using a rule set specified in the handler's configuration. For example, if the method accepts a custom type such as **Customer** or **Order** as a parameter, the handler will look for a rule set defined within that custom class and validate the instance accordingly. The **SpecificationSource** property controls where the handler will look for custom rule sets.
+ It validates the parameters according to the discovered validators. 
+ If validation succeeds, it allows the next handler to execute.
+ If validation fails, it creates an exception, wraps it in a message, and returns it to the previous handler, which may act on it. The exception ultimately returns to the caller.
<a name="_Toc253065329" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

> ![(../../../images/note.gif)Note:
> The validation handler will always initialize the Validation Application Block using the default configuration source, even if you instantiate the handler yourself in code and specify an alternative configuration source. 


# Creating Instances of the Validation Handler #
When you use this call handler with the Unity interception mechanism, you must provide values for any mandatory parameters of its constructors, and optionally provide values for other parameters. These values are used to set the properties of the handler at run time. The constructors you can use are shown in the following code.  

```csharp
ValidationCallHandler(string ruleSet, SpecificationSource specificationSource)

ValidationCallHandler(string ruleSet, SpecificationSource specificationSource, 
int handlerOrder)

ValidationCallHandler(string ruleSet, ValidatorFactory factory, int handlerOrder)

```


```vb
ValidationCallHandler(ruleSet As String, specificationSource As SpecificationSource)

ValidationCallHandler(ruleSet As String, specificationSource As SpecificationSource, handlerOrder As Integer)

ValidationCallHandler(ruleSet As String, factory As ValidatorFactory, handlerOrder As Integer)

```

The following table describes the values for the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>ruleSet</b></p></td><td><p><b>String</b>. The name of the rule set to use for all target object types. An empty string causes the handler to use the default rule set.</p></td></tr><tr><td><p><b>specificationSource</b></p></td><td><p>A value from the <b>SpecificationSource</b> enumeration that defines the locations where the handler will look for validation rules. Valid values are <b>Attributes</b>, <b>Configuration</b>, <b>ParameterAttributesOnly</b>, and <b>Both</b>.</p></td></tr><tr><td><p><b>order</b></p></td><td><p><b>Integer</b>. The position of the handler within the policy handler chain, starting from <b>1</b>. The default value is zero, which means that there is no explicit order specified for the handler in relation to other handlers in the same handler chain.</p></td></tr></table>
The validation handler exposes only the **Order** property.   
The following code extract shows how you can add a validation handler to a policy using the Unity interception mechanism.   

```csharp
myContainer.Configure<Interception>()
           .AddPolicy("MyPolicy")
               .AddMatchingRule<TypeMatchingRule>
                    (new InjectionConstructor("My.Order.Object", true))
               .AddCallHandler<ValidationCallHandler>
                   ("MyValidator", new ContainerControlledLifetimeManager());
```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
               .AddMatchingRule(Of TypeMatchingRule) _
                   (New InjectionConstructor("My.Order.Object", True))
               .AddCallHandler(Of ValidationCallHandler)("MyValidator", New ContainerControlledLifetimeManager())
```


# Using the Validation Handler Attribute #
Instead of configuring a call handler as part of a call hander pipeline, you force it to be applied by adding the appropriate attribute to your classes. The following code shows the use of the **ValidationCallHandler** attribute on a simple method. This attribute can also be applied to the class declaration, in which case it applies to all members of that class. The attribute accepts a parameter containing the name of the rule set to use for validation, as defined in the application configuration. If omitted, the handler uses the default validation rule set. The following example shows both of these scenarios.  

```csharp
[ValidationCallHandler("ruleset-name")]
public void Deposit(decimal depositAmount)
{
  balance += depositAmount;
}

[ValidationCallHandler()]
public void Withdraw(decimal withdrawAmount)
{
  balance -= withdrawAmount;
}
```


```vb
<ValidationCallHandler("ruleset-name")> _
Public Sub Deposit(depositAmount As Decimal)
balance += depositAmount
End Sub

<ValidationCallHandler> _
Public Sub Withdraw(withdrawAmount As Decimal)
balance -= withdrawAmount
End Sub
```

The following table describes the properties of the **ValidationCallHandlerAttribute **class.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>SpecificationSource</b></p></td><td><p>A value from the <b>SpecificationSource</b> enumeration that defines the locations where the handler will look for validation rules. Valid values are <b>Attributes</b>, <b>Configuration</b>, <b>ParameterAttributesOnly</b>, and <b>Both</b>.</p></td></tr><tr><td><p><b>Order</b></p></td><td><p><b>Integer</b>. The position of the handler within the policy handler chain, starting from <b>1</b>. The default value is zero, which means that there is no explicit order specified for the handler in relation to other handlers in the same handler chain.</p></td></tr></table>
To set these properties using an attribute, add them as parameters to the attribute declaration, as shown in the following code.  

```csharp
[ValidationCallHandler("ruleset-name", SpecificationSource=SpecificationSource.Both])
```


```vb
<ValidationCallHandler("ruleset-name", SpecificationSource:=SpecificationSource.Both)>
```

<a name="_Toc253065331" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Validating Parameters with Attribute-based Targeting #
When you use the **Validation Call Handler**, you can specify the name of a rule set when using an **Object Validator **to validate individual parameters of a method, as shown in the following code.   

```csharp
[ValidationCallHandler]
public void SomeMethod(
            [RegexValidator("...")] String x,
            [ObjectValidator("RulesetName")] String y) 
{ 
  ... 
}
```


```vb
<ValidationCallHandler> _
Public Sub SomeMethod( _
           <RegexValidator("...")> x As String, _
           <ObjectValidator("RulesetName")> y As String) 
End Sub
```

For more information about using call handler attributes and attribute-driven policies, see [Attribute-Driven Policies](test-markdown_456aac54-4ba3-4904-adae-36fb5227fabc.html).  



