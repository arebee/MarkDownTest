---
Source File Name: 75-Interception.docx
AssetID: 412f3261-e0f5-4998-8373-5dc2ebda16af
Title: Policy Injection Matching Rules
Order In ToC: 2\6\1
Output Filename: 2\6\1_Policy Injection Matching Rules.markdown
---

#### Markdown Test ####
# Policy Injection Matching Rules #
----------


> ![](../../images/note.gif)#!155CharTopicSummary!#:
> 
Unity includes matching rule implementations that provide capabilities for selecting objects and their members to which it adds a handler pipeline.

Unity includes matching rule implementations that provide a wide range of capabilities for selecting the objects and their members to which Unity will add a handler pipeline. Interception policies use the matching rules to define which methods will be intercepted.   
A matching rule is essentially a predicate that Unity checks each time it intercepts object creation. If all of the specified matching rules evaluate to **True** for any particular invocation, the application block will create and add the handler pipeline for that policy. If any one of the matching rules does not evaluate to true, Unity generates an instance of the original object or a derived object, and does not create a proxy or a handler pipeline.  
The following table lists the matching rules provided with Unity, and summarizes their use and parameters.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Matching rule</p></th><th><p>Description</p></th></tr><tr><td><p><b>Assembly Matching Rule</b></p></td><td><p>Selects classes in a specified assembly.</p></td></tr><tr><td><p><b>Custom Attribute Matching Rule</b></p></td><td><p>Selects classes or members that have an arbitrary attribute applied.</p></td></tr><tr><td><p><b>Member Name Matching Rule</b></p></td><td><p>Selects class members based on the member name.</p></td></tr><tr><td><p><b>Method Signature Matching Rule</b></p></td><td><p>Selects class methods that have a specific signature.</p></td></tr><tr><td><p><b>Namespace Matching Rule</b></p></td><td><p>Selects classes based on the namespace name.</p></td></tr><tr><td><p><b>Parameter Type Matching Rule</b></p></td><td><p>Selects class members based on the type name of a parameter for a member of the target object.</p></td></tr><tr><td><p><b>Property Matching Rule</b></p></td><td><p>Selects class properties by name and accessor type.</p></td></tr><tr><td><p><b>Return Type Matching Rule</b></p></td><td><p>Selects class members that return an object of the specified type.</p></td></tr><tr><td><p><b>Tag Attribute Matching Rule</b></p></td><td><p>Selects class members that carry a <b>Tag</b> attribute with the specified name.</p></td></tr><tr><td><p><b>Type Matching Rule</b></p></td><td><p>Selects classes that are of a specified type.</p></td></tr></table>
The following sections describe the built-in matching rules in detail:  
+ [The Assembly Matching Rule](./1/1_The Assembly Matching Rule.markdown)
+ [The Custom Attribute Matching Rule](./1/2_The Custom Attribute Matching Rule.markdown)
+ [The Member Name Matching Rule](./1/3_The Member Name Matching Rule.markdown)
+ [The Method Signature Matching Rule](./1/4_The Method Signature Matching Rule.markdown)
+ [The Namespace Matching Rule](./1/5_The Namespace Matching Rule.markdown)
+ [The Parameter Type Matching Rule](./1/6_The Parameter Type Matching Rule.markdown)
+ [The Property Matching Rule](./1/7_The Property Matching Rule.markdown)
+ [The Return Type Matching Rule](./1/8_The Return Type Matching Rule.markdown)
+ [The Tag Attribute Matching Rule](./1/9_The Tag Attribute Matching Rule.markdown)
+ [The Type Matching Rule](./1/10_The Type Matching Rule.markdown)
You can also create and use custom matching rules. For more information, see [Creating Policy Injection Matching Rules](http://msdn.microsoft.com/library/17137b99-e7a8-4944-b784-87db6ab429af.html). For information about using interception, see [Using Interception and Policy Injection](test-markdown_7a2c7fa6-28c2-479e-8df9-b4651824eb94).  


