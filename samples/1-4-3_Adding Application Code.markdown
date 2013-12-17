---
Source File Name: 40-Logging.docx
AssetID: 730d69d7-7e0f-4b21-8ab8-725bcec1bfd3
Title: Adding Application Code
Order In ToC: 1-4-3
Output Filename: 1-4-3_Adding Application Code.markdown
---

#### Markdown Test ####
# Adding Application Code #
----------

The Logging Application Block is designed to support the most common scenarios for logging information. When adding your application code, refer to the scenarios in the <a href="test-markdown_33d9998d-fa92-40b4-be49-7e28a72bd22c.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Key Scenarios</a> sections and select the ones that best suit your situation. Use the code that accompanies the scenario either as it is or adapt it as necessary.   
First, prepare your application to use the Logging Application Block. The following procedure describes how to include the necessary Enterprise Library assemblies and elements in your code.  
**To prepare your application**

1. Add a reference to the Logging Application Block assemblies. In Microsoft Visual Studio, right-click your project node in Solution Explorer, and then click **Manage NuGet Packages**. 
2. Click the **Online** button, and then in the **Search Online** box, type **EnterpriseLibrary.Logging**. 
3. Click the **Install** button for the **Enterprise Library 6.0 – Logging Application Block** package.
4. Read and accept the license terms for the packages listed.
5. After NuGet has finished installing the packages, click **Close**.
6. NuGet has now updated your project with all the necessary assemblies and references that you need to use the Logging Application Block.  
7. (Optional) If you plan to log to a database, use NuGet to install the **Enterprise Library 6.0 – Logging Application Block Database Provider** package (search for **EnterpriseLibrary.Logging.Database** in the NuGet package manager).
8. (Optional) If you plan to use remote logging, use NuGet to install the **Enterprise Library 6.0 – Remote Logging Service** package (search for **EnterpriseLibrary.Logging.Service** in the NuGet package manager).
9. (Optional) To use elements from the Logging Application Block without fully qualifying the element reference, you can add the following **using** statements (C#) or **Imports** statements (Visual Basic) to the top of your source code file.
```C#
using Microsoft.Practices.EnterpriseLibrary.Logging;
using Microsoft.Practices.EnterpriseLibrary.Logging.ExtraInformation;
using Microsoft.Practices.EnterpriseLibrary.Logging.Filters;
```


```Visual Basic
Imports Microsoft.Practices.EnterpriseLibrary.Logging
Imports Microsoft.Practices.EnterpriseLibrary.Logging.ExtraInformation
Imports Microsoft.Practices.EnterpriseLibrary.Logging.Filters
```

![](images/note.gif)Note:For Visual Basic projects, you can also use the **References** page of the Project Designer to manage references and imported namespaces. To access the **References** page, select a project node in Solution Explorer, and then click **Properties** on the **Project** menu. When the Project Designer appears, click the **References** tab.The **ExtraInformation** providers gather context information that is useful but not always necessary because it is expensive to collect. Examples are stack trace information and COM+ information. The **ExtraInformation** providers add the information to a dictionary. You can choose which providers to use (if any) and add the resulting dictionary to the **LogEntry.ExtendedProperties** property.  
Filters are optional. You only need to import the **Microsoft.Practices.EnterpriseLibrary.Logging.Filters** namespace if you are going to refer to specific filters in your application code.  

10. If you are using the **DatabaseTraceListener** class, you must also do the following:+ Configure the application to use the Data Access Application Block. For more information, see <a href="test-markdown_68577648-b6f9-478f-ad6a-953836e97c53.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">The Data Access Application Block</a>.
+ Execute the script named **CreateLoggingDb.cmd** (located in the [Solution Folder]\Packages\EnterpriseLibrary.Logging.Database.[Version]\Scripts folder) to create the **Logging** database. 

11. Create a **LogWriter** instance. For more information see <a href="test-markdown_875469ce-1185-4690-9d1c-36d452bf6a4a.html" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Creating LogWriter and TraceManager Instances</a>.
12. Add the application code. 
