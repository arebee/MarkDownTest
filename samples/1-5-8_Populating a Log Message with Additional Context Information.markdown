---
Source File Name: 40-Logging.docx
AssetID: 62843eda-e525-4531-8d26-4efddd75ccef
Title: Populating a Log Message with Additional Context Information
Order In ToC: 1-5-8
Output Filename: 1-5-8_Populating a Log Message with Additional Context Information.markdown
---

#### Markdown Test ####
# Populating a Log Message with Additional Context Information #
----------

The **LogEntry** class defines properties to hold information common to typical logging scenarios. Developers often need to add context information to log entries. The same type of context information can be required for multiple log entries in the same application or in multiple applications. Because context information can be expensive to gather, certain types of information are not automatically collected. The **ExtraInformation** providers gather context information that is useful but not always necessary because it is expensive to collect. Examples are stack trace information and COM+ information.   

# Typical Goals #
In this scenario, you want to add a collection of related properties to a log entry. The values of the properties are determined by the specific context of the application at the time the log entry is created. For example, you want to add security-related properties such as the Windows identity and authentication status of the current user.   

# Solution #
Use the application block classes that implement **IExtraInformationProvider** to populate a collection of properties. Use the **ExtendedProperties** property of the **LogEntry** class to pass the collection of properties to the **Write** method of the **LogWriter** class.  
The Logging Application Block includes the following classes that derive from **IExtraInformationProvider**:  
+ **ComPlusInformationProvider**. This class populates the collection with the following commonly needed COM+ diagnostic information: + Activity ID 
+ Application ID 
+ Transaction ID 
+ Direct caller account name 
+ Original caller account name 

+ **DebugInformationProvider**. This class populates the collection with a property containing the current stack trace information.
+ **ManagedSecurityContextInformationProvider**. This class populates the collection with the following commonly needed security-related information from the managed runtime:+ Authentication type 
+ Current identity name 
+ Authentication status 

+ **UnmanagedSecurityContextInformationProvider**. This class populates the collection with the following commonly needed unmanaged security-related information:+ Current user 
+ Process account name 

<a name="_Toc253065062" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Populating a Dictionary with Context Information #
The following code shows how to use the **ManagedSecurityContextInformationProvider** class to populate a dictionary with security-related information.   

```C#
Dictionary&lt;string, object&gt; dictionary = new Dictionary&lt;string, object&gt;();
ManagedSecurityContextInformationProvider informationHelper 
  = new ManagedSecurityContextInformationProvider();    
informationHelper.PopulateDictionary(dictionary);
```


```Visual Basic
Dim dictionary As New Dictionary(Of String, Object)()
Dim informationHelper As New ManagedSecurityContextInformationProvider()
informationHelper.PopulateDictionary(dictionary)
```

You can also add custom properties to the dictionary. The following code shows how to add the current screen resolution to the collection of properties declared in the previous example.   

```C#
int width = Screen.PrimaryScreen.Bounds.Width;
int height = Screen.PrimaryScreen.Bounds.Height;
string resolution = String.Format("{0}x{1}", width, height);
dictionary.Add("Screen resolution", resolution);
```


```Visual Basic
Dim width As Integer = Screen.PrimaryScreen.Bounds.Width
Dim height As Integer = Screen.PrimaryScreen.Bounds.Height
Dim resolution As String = String.Format("{0}x{1}", width, height)
dictionary.Add("Screen resolution", resolution)
```

The following code shows how to add the collection of properties to a **LogEntry** object stored in the variable named **myLogEntry**.   

```C#
myLogEntry.ExtendedProperties = dictionary;
```


```Visual Basic
myLogEntry.ExtendedProperties = dictionary
```


