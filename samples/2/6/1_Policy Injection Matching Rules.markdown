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


&gt; ![](/images/note.gif)#!155CharTopicSummary!#:
&gt; 
Unity includes matching rule implementations that provide capabilities for selecting objects and their members to which it adds a handler pipeline.
Unity includes matching rule implementations that provide a wide range of capabilities for selecting the objects and their members to which Unity will add a handler pipeline. Interception policies use the matching rules to define which methods will be intercepted.   
A matching rule is essentially a predicate that Unity checks each time it intercepts object creation. If all of the specified matching rules evaluate to **True** for any particular invocation, the application block will create and add the handler pipeline for that policy. If any one of the matching rules does not evaluate to true, Unity generates an instance of the original object or a derived object, and does not create a proxy or a handler pipeline.  
The following table lists the matching rules provided with Unity, and summarizes their use and parameters.   
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Matching rule</p></th><th><p>Description</p></th></tr><tr><td><p><b>Assembly Matching Rule</b></p></td><td><p>Selects classes in a specified assembly.</p></td></tr><tr><td><p><b>Custom Attribute Matching Rule</b></p></td><td><p>Selects classes or members that have an arbitrary attribute applied.</p></td></tr><tr><td><p><b>Member Name Matching Rule</b></p></td><td><p>Selects class members based on the member name.</p></td></tr><tr><td><p><b>Method Signature Matching Rule</b></p></td><td><p>Selects class methods that have a specific signature.</p></td></tr><tr><td><p><b>Namespace Matching Rule</b></p></td><td><p>Selects classes based on the namespace name.</p></td></tr><tr><td><p><b>Parameter Type Matching Rule</b></p></td><td><p>Selects class members based on the type name of a parameter for a member of the target object.</p></td></tr><tr><td><p><b>Property Matching Rule</b></p></td><td><p>Selects class properties by name and accessor type.</p></td></tr><tr><td><p><b>Return Type Matching Rule</b></p></td><td><p>Selects class members that return an object of the specified type.</p></td></tr><tr><td><p><b>Tag Attribute Matching Rule</b></p></td><td><p>Selects class members that carry a <b>Tag</b> attribute with the specified name.</p></td></tr><tr><td><p><b>Type Matching Rule</b></p></td><td><p>Selects classes that are of a specified type.</p></td></tr></table>
The following sections describe the built-in matching rules in detail:  
+ [The Assembly Matching Rule](test-markdown_4c47f30e-455d-4056-b773-69e6618c96fd.html)
+ [The Custom Attribute Matching Rule](test-markdown_e3c1e47a-2b21-4bff-8a1a-34f9191e4e65.html)
+ [The Member Name Matching Rule](test-markdown_78c97c0f-62c4-4008-81c2-858b34e954cc.html)
+ [The Method Signature Matching Rule](test-markdown_ebe602cf-d251-4bec-ad5c-d41bbef7550b.html)
+ [The Namespace Matching Rule](test-markdown_f5b9b0a8-66fd-4c47-b379-b49865ccc2c9.html)
+ [The Parameter Type Matching Rule](test-markdown_ff549bb6-e05a-4ea9-82e8-11516e1eafea.html)
+ [The Property Matching Rule](test-markdown_07573c82-9f99-4474-8e16-d2b8b6ea62a8.html)
+ [The Return Type Matching Rule](test-markdown_4b09e824-1e73-4230-988f-9a8ed8f5968a.html)
+ [The Tag Attribute Matching Rule](test-markdown_fc17bbda-834f-4f22-88f3-ccbba75bd917.html)
+ [The Type Matching Rule](test-markdown_f745650b-dd5f-4703-be36-7b3ece55cb19.html)
You can also create and use custom matching rules. For more information, see [Creating Policy Injection Matching Rules](test-markdown_17137b99-e7a8-4944-b784-87db6ab429af.html). For information about using interception, see [Using Interception and Policy Injection](test-markdown_7a2c7fa6-28c2-479e-8df9-b4651824eb94.html).  

