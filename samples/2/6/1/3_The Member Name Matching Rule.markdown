---
Source File Name: 75-Interception.docx
AssetID: 78c97c0f-62c4-4008-81c2-858b34e954cc
Title: The Member Name Matching Rule
Order In ToC: 2\6\1\3
Output Filename: 2\6\1\3_The Member Name Matching Rule.markdown
---

#### Markdown Test ####
# The Member Name Matching Rule #
----------


&gt; ![](/images/note.gif)#!155CharTopicSummary!#:
&gt; 
The member name matching rule allows you to select target classes based on the name of the class members (methods or properties).
The member name matching rule allows developers, operators, and administrators to select target classes based on the name of the class members (methods or properties), and allows you to use wildcard characters for the member name.  

# Behavior of the Member Name Matching Rule #
The member name matching rule does the following:  
+ It uses the value of the parameters passed to it to configure the matching rule for injection.
+ It compares each member name to match to the names of the members of the target object, taking into account any wildcard characters.
+ It performs the comparison on a non-case-sensitive basis if the **ignoreCase** parameter is **True** or on a case-sensitive basis if the **ignoreCase** property is **False**. 
+ It returns **True** if any of the member name to match values match a target object member name; if none match a target object member name, it returns **False**.
The matching rules for a policy can be defined in configuration or created and applied to policies at run time. For more information about configuring matching rules at design time, see [Configuration Files for Interception](test-markdown_af2f3726-4a3e-4e31-8f97-ebca0db3d907.html) in the section [Design-Time Configuration](test-markdown_d084d31d-6894-4cd3-ab6b-40f7a69899b2.html).  

# Creating a Member Name Matching Rule at Run Time #
The following constructor overloads can be used when creating an instance of the **MemberNameMatchingRule** class.  

```csharp
MemberNameMatchingRule(string nameToMatch)

MemberNameMatchingRule(string nameToMatch, bool ignoreCase)

MemberNameMatchingRule(IEnumerable&lt;string&gt; namesToMatch)

MemberNameMatchingRule(IEnumerable&lt;string&gt; namesToMatch, bool ignoreCase)

MemberNameMatchingRule(IEnumerable&lt;MatchingInfo&gt; matches)
```


```vb
MemberNameMatchingRule(nameToMatch As String)

MemberNameMatchingRule(nameToMatch As String, ignoreCase As Boolean)

MemberNameMatchingRule(namesToMatch As IEnumerable(Of String))

MemberNameMatchingRule(namesToMatch As IEnumerable(Of String), ignoreCase As Boolean)

MemberNameMatchingRule(matches As IEnumerable(Of MatchingInfo))
```

The following table describes the parameters shown above.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Parameter</p></th><th><p>Description</p></th></tr><tr><td><p><b>nameToMatch</b></p></td><td><p>This is the name of a method or parameter of the target object, such as GetOrderDetails. It can consist of or include the * or ? wildcard characters for selecting multiple types. You can also use square brackets [ ] to specify a range of characters. The following are some examples:</p><p>GetOrder*</p><p>*OrderFunctions</p><p>OrderProcess??Node</p><p>Transacted[order]Node</p><p>*</p></td></tr><tr><td><p><b>namesToMatch</b></p></td><td><p><b>String </b>collection. A list of more than one method or parameter name, using the same rules as for the <b>nameToMatch</b> parameter.</p></td></tr><tr><td><p><b>matches</b></p></td><td><p><b>MatchingInfo </b>collection. A list of one or more method or parameter names, using the same rules as for the <b>nameToMatch</b> parameter. <b>MatchingInfo</b> is a class used for storing information about a single name and case-sensitivity value pair.</p></td></tr><tr><td><p><b>ignoreCase</b></p></td><td><p><b>Boolean</b>. This specifies whether the match should be carried out on a case-sensitive basis. The default is false.</p></td></tr></table>
The following code excerpt shows how you can add a member name matching rule to a policy using the Unity interception mechanism.  

```csharp
myContainer.Configure&lt;Interception&gt;()
           .AddPolicy("MyPolicy")
           .AddMatchingRule&lt;MemberNameMatchingRule&gt;
                (new InjectionConstructor("MyMemberName", true))
           .AddCallHandler&lt;MyCallHandler&gt;
               ("MyValidator", 
               new ContainerControlledLifetimeManager());
```


```vb
myContainer.Configure(Of Interception)() _
           .AddPolicy("MyPolicy") _
           .AddMatchingRule(Of MemberNameMatchingRule) _
               (New InjectionConstructor("MyMemberName", True)) _
           .AddCallHandler(Of MyCallHandler) _
               ("MyValidator", New ContainerControlledLifetimeManager())

```

The code does not show how to create the container, add the Unity interception container extension, specify an interceptor, or resolve the intercepted target object. For more information about using matching rules with interception at run time, see [Registering Policy Injection Components](test-markdown_2090aa6d-38c7-4527-a211-aa4fa966e855.html).  

