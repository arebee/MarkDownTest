---
Source File Name: 75-Interception.docx
AssetID: f27ca9a4-3284-4917-91b9-f2b8c73f24f0
Title: The Authorization Handler
Order In ToC: 2\6\3\1
Output Filename: 2\6\3\1_The Authorization Handler.markdown
---

#### Markdown Test ####
# The Authorization Handler #
----------


> ![](../../../images/note.gif)#!155CharTopicSummary!#:
> 
This handler uses the Security Application Block and takes advantage of the features that it exposes. 

The authorization handler provides the capability to check that the current user (the security principal for the current thread) has the requisite permission to access the selected object method or property. This handler uses the Security Application Block and takes advantage of the features that it exposes.   
The authorization handler applies the security check **before** invocation of the selected method or setting of the selected property of the target object. If the current user does not have permission to access the method or property accessor, the authorization handler aborts execution during the preprocessing stage and does not invoke the target method or set the target property. It also generates an **UnauthorizedAccessException **and packages it into the message passed back to the previous handler in the pipeline.  


> ![](../../../images/note.gif)Note:
> This call handler is implemented in the Microsoft.Practices.EnterpriseLibrary.Security.PolicyInjection namespace of the Security Application Block in the Microsoft.Practices.EnterpriseLibrary.Security.dll assembly.


# Behavior of the Authorization Handler #
The authorization handler does the following:  
+ It uses the type of authorization provider specified, which maps to a configured authorization provider instance in the Security Application Block. 
+ It uses the **OperationName**, which may contain tokens for contextual items such as the type or method name (see the following table for a full list). 
+ It calls the specified authorization provider using the current thread principal when the handler is invoked. 
+ If the authorization succeeds, it allows the next handler to execute.
+ If the authorization fails, it returns an **UnauthorizedAccessException** to the caller and does not allow the next handler to execute. 

# Creating Instances of the Authorization Handler #
When you use this call handler with the Unity interception mechanism, you must provide values for any mandatory parameters of its constructors, and optionally provide values for other parameters. These values are used to set the properties of the handler at run time. The constructors you can use are shown in the following code.  

```csharp
AuthorizationCallHandler(IAuthorizationProvider provider, 
                         string operationName, 
                         int order)
```


```vb
AuthorizationCallHandler(provider As IAuthorizationProvider, _
                         operationName As String, _
                         order As Integer)
```

The following table describes the values for the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>provider</b></p></td><td><p><b>IAuthorizationProvider</b>. A reference to the authorization provider instance to use. You can resolve this through the Unity container by specifying the <b>IAuthorizationProvider</b> interface and the name of the required provider.</p></td></tr><tr><td><p><b>operationName</b></p></td><td><p><b>String</b>. The name of the authorization operation, which may include the tokens {method}, {type}, {namespace}, {assembly}, and {appdomain}.</p></td></tr><tr><td><p><b>order</b></p></td><td><p><b>Integer</b>. The position of the handler within the policy handler chain, starting from <b>1</b>. The default value is zero, which means that there is no explicit order specified for the handler in relation to other handlers in the same handler chain.</p></td></tr></table><a name="handlerconfigcache" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>
The authorization handler also exposes these values as the **AuthorizationProvider**, **OperationName**, and **Order** properties.   
The following code extract shows how you can add an authorization handler to a policy using the Unity type resolution and interception mechanisms.   

```csharp
IAuthorizationProvider myProvider 
    = myContainer.Resolve<IAuthorizationProvider>("MyAuthProvider");

myContainer.Configure<Interception>()
           .AddPolicy("MyPolicy")
               .AddMatchingRule<TypeMatchingRule>(new InjectionConstructor("My.Order.Object",
                        true))
           .AddCallHandler<AuthorizationCallHandler>
                ("MyAuthProvider", new ContainerControlledLifetimeManager());
```


```vb
Dim myProvider As IAuthorizationProvider = myContainer.Resolve(Of IAuthorizationProvider)("MyAuthProvider")

myContainer.Configure(Of Interception)() _
    .AddPolicy("MyPolicy") _
        .AddMatchingRule(Of TypeMatchingRule) _
            (New InjectionConstructor("My.Order.Object", True)) _
        .AddCallHandler(Of AuthorizationCallHandler) _
            ("MyAuthProvider", New ContainerControlledLifetimeManager())
```


# Using the Authorization Handler Attribute #
Instead of configuring a call handler as part of a call handler pipeline, you force it to be applied by adding the appropriate attribute to your classes. The call handler attribute for the authorization handler requires the provider name as the only mandatory parameter. You can set other properties of the handler attribute in order to configure the call handler that it creates. The following table lists the properties of the authorization handler attribute.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Property</p></th><th><p>Description</p></th></tr><tr><td><p><b>AuthorizationProvider</b></p></td><td><p><b>IAuthorizationProvider</b>. The authorization provider instance to use, as configured in the Security Application Block.</p></td></tr><tr><td><p><b>OperationName</b></p></td><td><p><b>String</b>. The name of the authorization operation, which may include the tokens {method}, {type}, {namespace}, {assembly}, and {appdomain}.</p></td></tr><tr><td><p><b>Order</b></p></td><td><p><b>Integer</b>. The position of the handler within the policy handler chain, starting from <b>1</b>. The default value is zero, which means that there is no explicit order specified for the handler in relation to other handlers in the same handler chain.</p></td></tr></table>
The following code shows the use of the **AuthorizationCallHandler** attribute on a simple method. This attribute can also be applied to the class declaration, in which case it applies to all members of that class. The operation name is a mandatory parameter.   

```csharp
[AuthorizationCallHandler("operation-name")]
public void Deposit(decimal depositAmount)
{
  balance += depositAmount;
}
```


```vb
<AuthorizationCallHandler("operation-name")> _
Public Sub Deposit(depositAmount As Decimal)
  balance += depositAmount
End Sub
```

You can also use a call handler attribute to specify the authorization provider name, as shown in the following code.  

```csharp
[AuthorizationCallHandler(OperationName="operation-name", ProviderName="provider-name")]
```


```vb
<AuthorizationCallHandler(OperationName:="operation-name", _
    ProviderName:="provider-name")>
```

For more information about using call handler attributes and attribute-driven policies, see [Attribute-Driven Policies](../2_Attribute-Driven Policies.markdown).  


