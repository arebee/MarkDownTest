---
Source File Name: 75-Interception.docx
AssetID: 73690eea-cce2-48aa-a143-d6fcd80e1654
Title: Behaviors for Interception
Order In ToC: 2\2
Output Filename: 2\2_Behaviors for Interception.markdown
---

#### Markdown Test ####
# Behaviors for Interception #
----------


> ![(../images/note.gif)#!155CharTopicSummary!#:
> 
Interception is based on a behavior or series of behaviors in the behaviors pipeline that describe what to do when an object is intercepted.

Interception is based on a behavior or series of behaviors in the behaviors pipeline that describe what to do when an object is intercepted. Unity provides a built-in default policy injection behavior to facilitate the implementation of policy injection. The policy injection behavior attaches or injects some functionality to specific methods by using call handlers and matching rules on a per-method basis. For more information on policy injection see [Using Interception and Policy Injection](test-markdown_7a2c7fa6-28c2-479e-8df9-b4651824eb94.html).  
You can also create your own custom behaviors by implementing the **IInterceptionBehavior** interface. The interception behaviors are added to a pipeline and are called for each invocation of that pipeline. You have wide latitude in what functionality you design for your behavior. Some practical uses of behaviors include implementing custom tasks and business rules, implementing **INotifyPropertyChanged **to support a property change event, to provide support for the **ErrorProvider/IDataErrorInfo** approach to validation in Windows Presentation Foundation (WPF), and to implement a **Mocking** framework.  
Custom tasks and business rules you might chose to implement with individual custom behaviors would include tasks such as validating parameters or authorizing users.  
When implementing a WPF view model, in order to get the property change event action you must implement **INotifyPropertyChanged**. Rather than implementing this every time, you can create a behavior that adds and implements the interface.   
There is no built-in support for the **ErrorProvider/IDataErrorInfo** approach to validation in WPF; hence there is no **ErrorProvider** component in WPF either. You must create an **ErrorProvider** for use in WPF applications. The **DataGrid** (1.1) and **DataGridView** (2.0) in Windows Forms both automatically detected the presence of this interface on objects they were bound to, and showed any errors without any work. The Windows Forms **ErrorProvider** could be used to automatically display errors on any control that came from the objects they (and the **ErrorProvider**) were bound to, all without any extra code being written. You can use **IDataErrorInfo** to take advantage of the validation work in the .NET Framework. You can implement **IDataErrorInfo** in a class and bind the class concrete object to the **DataGridView** control through the **Bindingsource **property. Then use the Validation Application Block to validate the class object and store the results for each property.  
Implementing a mocking framework through an interception behavior could be useful in cases where the code requires an interface that has no implementation. The behavior can give you a mock up interface. You could use a mocking framework to implement a mock database, mock logger, or mock builder context.   
The following topics explain or demonstrate interception behaviors in more detail:  
+ <a href="#interception_behavior_custom" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Custom Interception Behaviors</a>
+ <a href="#interception_behavior_INotifyProperty" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Implement** **IDataErrorInfo** **Example</a>

# Custom Interception Behaviors #
<a name="interception_behavior_custom" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>Custom interception behaviors are implementations of the **IInterceptionBehavior** interface. You must provide an implementation of the two **IInterceptionBehavior** interface methods, **Invoke** and **GetRequiredInterfaces,** and set the **WillExecute** property. The **Invoke** method is required to actually execute the behavior logic. For more information on injection attributes, see [Annotating Objects for Constructor Injection](test-markdown_fdc0d23e-7821-4510-90a8-a6a64bf51854.html). The **GetRequiredInterfaces **method returns the interfaces required by the behavior for the intercepted objects.  
The **WillExecute** property is simply used to optimize proxy creation. It simply returns a flag indicating if the behavior will actually do anything when invoked, and if not, enables the interception mechanism to skip the behavior. In the case of policy injection, when no policies match, **WillExecute** is false. If **WillExecute** is false for all the behaviors in the behaviors pipeline, then nothing is intercepted and proxy or intercepting class generation is simply not performed, thus optimizing performance.  

> ![(../images/note.gif)Note:
> If behaviors always return true and there is no action to perform, the only result is that you have the unnecessary overhead of the proxy or intercepting class generation.


# Implement INotifyPropertyChanged Example #
<a name="interception_behavior_INotifyProperty" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>The following **NotifyPropertyChangedBehavior** class is an example that implements **IInterceptionBehavior**.   

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Reflection;
using Microsoft.Practices.Unity.InterceptionExtension;

namespace InterceptionDemo.InterceptionBehaviors
{
    class NotifyPropertyChangedBehavior : IInterceptionBehavior
    {
        private event PropertyChangedEventHandler propertyChanged;

        private static readonly MethodInfo addEventMethodInfo =
            typeof (INotifyPropertyChanged).GetEvent("PropertyChanged").GetAddMethod();

        private static readonly MethodInfo removeEventMethodInfo =
            typeof (INotifyPropertyChanged).GetEvent("PropertyChanged").GetRemoveMethod();

        /// &lt;summary>
        /// Implement this method to execute your behavior processing.
        /// &lt;/summary>
        /// &lt;param name="input">Inputs to the current call to the target.&lt;/param>&lt;param name="getNext">Delegate to execute to get the next delegate in the behavior chain.&lt;/param>
        /// &lt;returns>
        /// Return value from the target.
        /// &lt;/returns>
        public IMethodReturn Invoke(IMethodInvocation input, GetNextInterceptionBehaviorDelegate getNext)
        {
            if(input.MethodBase == addEventMethodInfo)
            {
                return AddEventSubscription(input, getNext);
            }
            if(input.MethodBase == removeEventMethodInfo)
            {
                return RemoveEventSubscription(input, getNext);
            }
            if(IsPropertySetter(input))
            {
                return InterceptPropertySet(input, getNext);
            }
            return getNext()(input, getNext);
        }

        /// &lt;summary>
        /// Optimization hint for proxy generation - will this behavior actually
        /// perform any operations when invoked?
        /// &lt;/summary>
        public bool WillExecute
        {
            get { return true; }
        }

        /// &lt;summary>
        /// Returns the interfaces required by the behavior for the objects it intercepts.
        /// &lt;/summary>
        /// &lt;returns>
        /// The required interfaces.
        /// &lt;/returns>
        public IEnumerable&lt;Type> GetRequiredInterfaces()
        {
            return new [] { typeof(INotifyPropertyChanged) };
        }

        private IMethodReturn AddEventSubscription(IMethodInvocation input, GetNextInterceptionBehaviorDelegate getNext)
        {
            var subscriber = (PropertyChangedEventHandler) input.Arguments[0];
            propertyChanged += subscriber;
            return input.CreateMethodReturn(null);
        }

        private IMethodReturn RemoveEventSubscription(IMethodInvocation input, GetNextInterceptionBehaviorDelegate getNext)
        {
            var subscriber = (PropertyChangedEventHandler)input.Arguments[0];
            propertyChanged -= subscriber;
            return input.CreateMethodReturn(null);
        }

        private static bool IsPropertySetter(IMethodInvocation input)
        {
            return input.MethodBase.IsSpecialName &amp;&amp; input.MethodBase.Name.StartsWith("set_");
        }

        private IMethodReturn InterceptPropertySet(IMethodInvocation input, GetNextInterceptionBehaviorDelegate getNext)
        {
            var propertyName = input.MethodBase.Name.Substring(4);

            var returnValue = getNext()(input, getNext);

            var subscribers = propertyChanged;
            if(subscribers != null)
            {
                subscribers(input.Target, new PropertyChangedEventArgs(propertyName));
            }

            return returnValue;
        }
    }
}
```



```vb
    Public Class NotifyPropertyChangedBehavior
        Implements IInterceptionBehavior

        Private Event propertyChanged As PropertyChangedEventHandler

        Private Shared addEventMethodInfo As MethodInfo = _
            GetType(INotifyPropertyChanged).GetEvent("PropertyChanged").GetAddMethod()

        Private Shared removeEventMethodInfo As MethodInfo = _
            GetType(INotifyPropertyChanged).GetEvent("PropertyChanged").GetRemoveMethod()

        ''' &lt;summary>
        ''' Implement this method to execute your behavior processing.
        ''' &lt;/summary>
        ''' &lt;param name="input">Inputs to the current call to the target.&lt;/param>&lt;param name="getNext">Delegate to execute to get the next delegate in the behavior chain.&lt;/param>
        ''' &lt;returns>
        ''' Return value from the target.
        ''' &lt;/returns>
        Public Function Invoke(ByVal input As IMethodInvocation, _
            ByVal getNext As GetNextInterceptionBehaviorDelegate) _
            As IMethodReturn Implements IInterceptionBehavior.Invoke

            If ReferenceEquals(input.MethodBase, addEventMethodInfo) Then
                Return AddEventSubscription(input, getNext)
            End If
            If ReferenceEquals(input.MethodBase, removeEventMethodInfo) Then
                Return RemoveEventSubscription(input, getNext)
            End If
            If IsPropertySetter(input) Then
                Return InterceptPropertySet(input, getNext)
            End If

            Return getNext()(input, getNext)
        End Function
        ''' &lt;summary>
        ''' Returns the interfaces required by the behavior for the objects it intercepts.
        ''' &lt;/summary>
        ''' &lt;returns>
        ''' The required interfaces.
        ''' &lt;/returns>
        Public Function GetRequiredInterfaces() As IEnumerable(Of Type) _
        Implements IInterceptionBehavior.GetRequiredInterfaces

            Return New Type() {GetType(INotifyPropertyChanged)}
        End Function
        ''' &lt;summary>
        ''' Optimization hint for proxy generation - will this behavior actually
        ''' perform any operations when invoked?
        ''' &lt;/summary>
        Public ReadOnly Property WillExecute() As Boolean _
            Implements IInterceptionBehavior.WillExecute

            Get
                Return True
            End Get

        End Property

        Private Function AddEventSubscription(ByRef input As IMethodInvocation, _
            ByRef getNext As GetNextInterceptionBehaviorDelegate) As IMethodReturn

            Dim subscriber = DirectCast(input.Arguments(0), PropertyChangedEventHandler)
            AddHandler propertyChanged, subscriber
            Return input.CreateMethodReturn(Nothing)
        End Function

        Private Function RemoveEventSubscription(ByRef input As IMethodInvocation, _
            ByRef getNext As GetNextInterceptionBehaviorDelegate) As IMethodReturn

            Dim subscriber = DirectCast(input.Arguments(0), PropertyChangedEventHandler)
            RemoveHandler propertyChanged, subscriber
            Return input.CreateMethodReturn(Nothing)
        End Function

        Private Function IsPropertySetter(ByRef input As IMethodInvocation) As Boolean
            Return input.MethodBase.IsSpecialName AndAlso input.MethodBase.Name.StartsWith("set_")
        End Function

        Private Function InterceptPropertySet(ByVal input As IMethodInvocation, _
            ByVal getNext As GetNextInterceptionBehaviorDelegate) As IMethodReturn

            Dim propertyName = input.MethodBase.Name.Substring(4)

            Dim returnValue = getNext()(input, getNext)

            RaiseEvent propertyChanged(input.Target, New PropertyChangedEventArgs(propertyName))

            Return returnValue
        End Function
    End Class
```



