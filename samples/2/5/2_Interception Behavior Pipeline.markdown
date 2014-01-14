---
Source File Name: 75-Interception.docx
AssetID: 7f7a1362-150a-417c-8b15-ba4f7d08cd5a
Title: Interception Behavior Pipeline
Order In ToC: 2\5\2
Output Filename: 2\5\2_Interception Behavior Pipeline.markdown
---

#### Markdown Test ####
# Interception Behavior Pipeline #
----------


> ![](../../images/note.gif)#!155CharTopicSummary!#:
> <a name="interception_pipeline" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>
The pipeline maintains a list of interception behaviors and manages them, calling them in the proper order with the correct inputs.

You must add behaviors to the behavior pipeline to use them. The pipeline maintains a list of interception behaviors and manages them, calling them in the proper order with the correct inputs.  
The following example adds a behavior and interceptor to a new interception behaviors pipeline.  
First create an interception behaviors pipeline.  

```csharp
private InterceptionBehaviorPipeline pipeline = new InterceptionBehaviorPipeline();
```


```vb
Private pipeline As New InterceptionBehaviorPipeline()
```

Then add an interception behavior to the pipeline.  

```csharp
pipeline.Add(interceptor);
```


```vb
pipeline.Add(interceptor)
```



