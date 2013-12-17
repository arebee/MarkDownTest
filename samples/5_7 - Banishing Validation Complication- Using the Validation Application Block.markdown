---
Source File Name: C:\code\pandp\git\markdown\sampleDocx\60-Validation.docx
AssetID: 7475df36-8932-4d6a-95a4-de51cbad5f87
Title: 7 - Banishing Validation Complication: Using the Validation Application Block
Order In ToC: 5
Output Filename: 5_7 - Banishing Validation Complication- Using the Validation Application Block.markdown
---

#### Markdown Test ####
# 7 - Banishing Validation Complication: Using the Validation Application Block #
----------

<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!155CharTopicSummary!#:</th></tr><tr><td>
The Validation block is a highly flexible solution that allows you to specify validation rules in configuration, with attributes, or in code.</td></tr></table><p /></div>

# Introduction #
If you happen to live in the U.S. and I told you that the original release date of version 2.0 of Enterprise Library was 13/01/2006, you'd wonder if I'd invented some new kind of calendar. Perhaps they added a new month to the calendar without you noticing (which I'd like to call Plutember in honor of the now-downgraded ninth planet). Of course, in many other countries around the world, 13/01/2006 is a perfectly valid date in January. This validation issue is well known and the solution probably seems obvious, but I once worked with an application that used dates formatted in the U.S. mm/dd/yyyy pattern for the filenames of reports it generated, and defaulted to opening the report for the previous day when you started the program. Needless to say, on my machines set up to use U.K. date formats, it never did manage to find the previous day's report.  
Even better, when I tried to submit a technical support question on their Web site, it asked me for the date I purchased the software. The JavaScript validation code in the Web page running on my machine checked the format of my answer (27/04/2008) and accepted it. But the server refused to believe that there are twenty seven months in a year, and blocked my submission. I had to lie and say I purchased it on May 1 instead.  
The problem is that validation can be an onerous task, especially when you need to do it in so many places in your applications, and for a lot of different kinds of values. It's extremely easy to end up with repeated code scattered throughout your classes, and yet still leave holes where unexpected input can creep into your application and possibly cause havoc.   
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Jana)!#:</th></tr><tr><td>Validation is a great example of a cross-cutting concern. You need to validate in many different areas of the application and perform the same validation in multiple locations.</td></tr></table><p /></div>Robust validation can help to protect your application against malicious users and dangerous input (including SQL injection attacks), ensure that it processes only valid data and enforces business rules, and improve responsiveness by detecting invalid data before performing expensive processing tasks.  
So, how do you implement comprehensive and centralized validation in your applications? One easy solution is to take advantage of the Enterprise Library Validation Block. The Validation block is a highly flexible solution that allows you to specify validation rules in configuration, with attributes, or in code, and have that validation applied to objects, method parameters, fields, and properties. It even includes features that integrate with Windows Forms, Windows Presentation Foundation (WPF), ASP.NET, and Windows Communication Foundation (WCF) applications to present validation errors within the user interface or have them handled automatically by the service.   
<a name="_Toc254706753" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Techniques for Validation #
Before we explore the Validation block, it's worth briefly reviewing some validation good practices. In general, there are three factors you should consider: where you are going to perform validation, what data should you validate, and how you will perform this validation.  
<a name="_Toc254706754" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Where Should I Validate? S##
Validation should, of course, protect your entire application. However, it is often the case that you need to apply validation in more than one location. If your application consists of layers, distributed services, or discrete components, you probably need to validate at each boundary. This is especially the case where individual parts of the application could be called from more than one place (for example, a business layer that is used by several user interfaces and other services).   
It is also a really good idea to validate at trust boundaries, even if the components on each side of the boundary are not physically separated. For example, your business layer may run under a different trust level or account context than your data layer (even if they reside on the same machine). Validation at this boundary can prevent code that is running in low trust and which may have been compromised, from submitting invalid data to code that runs in higher trust mode.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Jana)!#:</th></tr><tr><td>Use trust and layer boundaries in your application to identify where to add validation.</td></tr></table><p /></div>Finally, a common scenario: validation in the user interface. Validating data on the client can improve application responsiveness, especially if the UI is remote from the server. Users do not have to wait for the server to respond when they enter or submit invalid data, and the server does not need to attempt to process data that it will later reject. However, remember that even if you do validate data on the client or in the UI you must always revalidate on the server or in the receiving service. This protects against malicious users who may circumvent client-side validation and submit invalid data.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Markus)!#:</th></tr><tr><td>Validation in the UI illustrates how you can use validation to make your application more efficient. There is no point in submitting a request that will travel through several layers (possibly crossing a network) if you can identify what’s wrong with the request in advance.</td></tr></table><p /></div><a name="_Toc254706755" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## What Should I Validate? S##
To put it simply, everything. Or, at least any input values you will use in your application that may cause an error, involve a security risk, or could result in incorrect processing. Remember that Web page and service requests may contain data that the user did not enter directly, but could be used in your application. This can include cookies, header information, credentials, and context information that the server may use in various ways. Treat all input data as suspicious until you have validated it.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Poe)!#:</th></tr><tr><td>Validate data from other systems as well as user input.</td></tr></table><p /></div><a name="_Toc254706756" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## How Should I Validate? S##
For maximum security, your validation process should be designed to accept only data that you can directly determine to be valid. This approach is known as positive validation and generally uses an allow list that specifies data that satisfies defined criteria, and rejects all other data. Examples are rules that check if a number is between two predefined limits, or if the submitted value is within a list of valid values. Use this approach whenever possible.   
The alternative and less-secure approach is to use a block list containing values that are not valid. This is called negative validation, and generally involves accepting only data that does not meet specific criteria. For example, as long as a string does not contain any of the specified invalid characters, it would be accepted. You should use this approach cautiously and as a secondary line of defense, because it is very difficult to create a complete list of criteria for all known invalid input—which may allow malicious data to enter your system.   
Finally, consider sanitizing data. While this is not strictly a validation task, you can as an extra precaution attempt to eliminate or translate characters in an effort to make the input safe. However, do not rely on this technique alone because, as with negative validation, it can be difficult to create a complete list of criteria for all known invalid input unless there is a limited range of invalid values.   
<a name="_Toc229824761" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc254706757" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc228863737" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc229213683" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc229824766" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# What Does the Validation Block Do? #
The Validation block consists of a broad range of validators, plus a mechanism that executes these validators and collects and correlates the results to provide an overall validation result (true/valid or false/invalid). The Validation block can use individual attributes applied to classes and class members that the application uses (both the validation attributes provided with the Validation block and data annotation attributes from the **System.ComponentModel.DataAnnotations **namespace), in addition to rule sets defined in the configuration of the block, which specify the validation rules to apply.   
The typical scenario when using the Validation block is to define rule sets through configuration or attributes applied to your classes. Each rule set specifies the set of individual validators and combinations of these validators that implement the validation rules you wish to apply to that class. Then you use a **ValidationFactory** (or one of the equivalent implementations of this factory) to create a type validator for the class, optionally specifying the rule set it should use. If you don’t specify a rule set, it uses the default rules. Then you can call the **Validate** method of the type validator. This method returns an instance of the **ValidationResults** class that contains details of all the validation errors detected. Figure 1 illustrates this process.  
<p class="label" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><div class="caption">Figure 1 - An overview of the validation process</div></p><img src="images\A6E0F4B4F21C668EB085D59DDAB24034.png" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp" />  
   
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Jana)!#:</th></tr><tr><td>Defining the validation in configuration makes it easier to modify the validation rules later.</td></tr></table><p /></div>When you use a rule set to validate an instance of a specific type or object, the block can apply the rules to:  
+ The type itself 
+ The values of public readable properties
+ The values of public fields
+ The return values of public methods that take no parameters
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>Notice that you can validate the values of method parameters and the return type of methods that take parameters when that method is invoked,<i> </i>only by using the validation call handler (which is part of the Policy Injection Application Block) in conjunction with the Unity dependency injection and interception mechanism. The validation call handler will validate the parameter values based on the rules for each parameter type and any validation attributes applied to the parameters. We don’t cover the use of the validation call handler in this guide, as it requires you to be familiar with Unity interception techniques. For more information about interception and the validation call handler, see Chapter 5, "<a href="http://msdn.microsoft.com/en-us/library/dn178466(v=pandp.30).aspx">Interception using Unity</a>" in the Developer's Guide to Dependency Injection Using Unity. </td></tr></table><p /></div>Alternatively, you can create individual validators programmatically to validate specific values, such as strings or numeric values. However, this is not the main focus of the block—though we do include samples in this chapter that show how you can use individual validators.  
In addition, the Validation block contains features that integrate with Windows Forms, Windows Presentation Foundation (WPF), ASP.NET, and Windows Communication Foundation (WCF) applications. These features use a range of different techniques to connect to the UI, such as a proxy validator class based on the standard ASP.NET **Validator** control that you can add to a Web page, a **ValidationProvider** class that you can specify in the properties of Windows Forms controls, a **ValidatorRule** class that you can specify in the definition of WPF controls, and a behavior extension that you can specify in the **&lt;system.ServiceModel&gt;** section of your WCF configuration. You'll see more details of these features later in this chapter.  
<a name="_Toc254706758" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## The Range of Validators S##
Validators implement functionality for validating Microsoft .NET Framework data types. The validators included with the Validation block fall into three broad categories: value validators, composite validators, and type (object) validators. The value validators allow you to perform specific validation tests such as verifying:  
+ The length of a string, or the occurrence of a specified set of characters within it.
+ Whether a value lies within a specified range, including tests for dates and times relative to a specified date/time.
+ Whether a value is one of a specified set of values, or can be converted to a specific data type or enumeration value.
+ Whether a value is null, or is the same as the value of a specific property of an object.
+ Whether the value matches a specified regular expression. 
The composite validators are used to combine other validators when you need to apply more complex validation rules. The Validation block includes the composite **AND** and **OR** validators, each of which acts as a container for other validators. By nesting these composite validators in any combination and populating them with other validators, you can create very comprehensive and very specific validation rules.   
Table 1 describes the complete set of validators provided with the Validation block.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Validator type</p></th><th><p>Validator name</p></th><th><p>Description</p></th></tr><tr><td rowspan="11"><p><b>Value Validators</b></p></td><td><p>Contains Characters Validator</p></td><td><p>Checks that an arbitrary string, such as a string entered by a user in a Web form, contains any or all of the specified characters.</p></td></tr><tr><td><p>Date Time Range Validator </p></td><td><p>Checks that a DateTime object falls within a specified range.</p></td></tr><tr><td><p>Domain Validator </p></td><td><p>Checks that a value is one of the specified values in a specified set.</p></td></tr><tr><td><p>Enum Conversion Validator </p></td><td><p>Checks that a string can be converted to a value in a specified enumeration type.</p></td></tr><tr><td><p>Not Null Validator </p></td><td><p>Checks that the value is not null.</p></td></tr><tr><td><p>Property Comparison Validator</p></td><td><p>Compares the value to be checked with the value of a specified property.</p></td></tr><tr><td><p>Range Validator </p></td><td><p>Checks that a value falls within a specified range.</p></td></tr><tr><td><p>Regular Expression Validator </p></td><td><p>Checks that the value matches the pattern specified by a regular expression.</p></td></tr><tr><td><p>Relative Date Time Validator </p></td><td><p>Checks that the DateTime value falls within a specified range using relative times and dates.</p></td></tr><tr><td><p>String Length Validator </p></td><td><p>Checks that the length of the string is within the specified range.</p></td></tr><tr><td><p>Type Conversion Validator </p></td><td><p>Checks that a string can be converted to a specific type.</p></td></tr><tr><td rowspan="2"><p><b>Type</b></p><p><b>Validators</b></p></td><td><p>Object Validator </p></td><td><p>Causes validation to occur on an object reference. All validators defined for the object's type will be invoked.</p></td></tr><tr><td><p>Object Collection Validator</p></td><td><p>Checks that the object is a collection of the specified type and then invokes validation on each element of the collection.</p></td></tr><tr><td rowspan="2"><p><b>Composite Validators</b></p></td><td><p>And Composite Validator </p></td><td><p>Requires all validators that make up the composite validator to be true.</p></td></tr><tr><td><p>Or Composite Validator </p></td><td><p>Requires at least one of the validators that make up the composite validator be true.</p></td></tr><tr><td rowspan="3"><p><b>Single Member Validators</b></p></td><td><p>Field Value Validator</p></td><td><p>Validates a field of a type.</p></td></tr><tr><td><p>Method Return Value Validator</p></td><td><p>Validates the return value of a method of a type.</p></td></tr><tr><td><p>Property Value Validator</p></td><td><p>Validates the value of a property of a type.</p></td></tr></table><p class="label" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><div class="caption">Table 1 - The validators provided with the Validation block</div></p>For more details on each validator, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>. You will see examples that use many of these validators throughout this chapter.  
<a name="_Toc254706759" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Validating with Attributes S##
If you have full access to the source code of your application, you can use attributes within your classes to define your validation rules. You can apply validation attributes in the following ways:  
+ To a **field**. The Validation block will check that the field value satisfies all validation rules defined in validators applied to the field.
+ To a **property**. The Validation block will check that the value of the get property satisfies all validation rules defined in validators applied to the property.
+ To a **method that takes no parameters**. The Validation block will check that the return value of the method satisfies all validation rules defined in validators applied to the method.
+ To an entire **class**, using only the **NotNullValidator**, **ObjectCollectionValidator**, **AndCompositeValidator**, and** OrCompositeValidator**). The Validation block can check if the object is null, that it is a member of the specified collection, and that any validation rules defined within it are satisfied.
+ To a **parameter in a WCF Service Contract**. The Validation block will check that the parameter value satisfies all validation rules defined in validators applied to the parameter.
+ To **parameters of methods that are intercepted**, by using the validation call handler in conjunction with the Policy Injection Application Block. For more information on using interception, see "<a href="http://msdn.microsoft.com/en-us/library/dn178466(v=pandp.30).aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Chapter 5 - Interception using Unity</a>" in the Unity Developer’s Guide.
Each of the validators described in the previous section has a related attribute that you apply in your code, specifying the values for validation (such as the range or comparison value) as parameters to the attribute. For example, you can validate a property that must have a value between 0 and 10 inclusive by applying the following attribute to the property definition, as seen in the following code.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[RangeValidator(0, RangeBoundaryType.Inclusive, 10, RangeBoundaryType.Inclusive)] </pre></td></tr>
            </table>
          </span>
        </div>
      <a name="_Toc254706760" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### DataAnnotations Attributes ###
In addition to using the built-in validation attributes, the Validation block will perform validation defined in the vast majority of the validation attributes in the **System.ComponentModel.DataAnnotations** namespace. These attributes are typically used by frameworks and object/relational mapping (O/RM) solutions that auto-generate classes that represent data items. They are also generated by the ASP.NET validation controls that perform both client-side and server-side validation. While the set of validation attributes provided by the Validation block does not map exactly to those in the **DataAnnotations** namespace, the most common types of validation are supported. A typical use of data annotations is shown here.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[System.ComponentModel.DataAnnotations.Required(
        ErrorMessage = "You must specify a value for the product ID.")]
[System.ComponentModel.DataAnnotations.StringLength(6, 
        ErrorMessage = "Product ID must be 6 characters.")]
[System.ComponentModel.DataAnnotations.RegularExpression("[A-Z]{2}[0-9]{4}",
        ErrorMessage = "Product ID must be 2 capital letters and 4 numbers.")]
public string ID { get; set; }</pre></td></tr>
            </table>
          </span>
        </div>
      In reality, the Validation block validation attributes <i>are</i> data annotation attributes, and can be used (with some limitations) whenever you can use data annotations attributes—for example, with ASP.NET Dynamic Data applications. The main difference is that the Validation block attribute validation occurs only on the server, and not on the client.   
Also keep in mind that, while DataAnnotations supports most of the Validation block attributes, not all of the validation attributes provided with the Validation block are supported by the built-in .NET validation mechanism. For more information, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>, and the topic "<a href="http://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">System.ComponentModel.DataAnnotations Namespace</a>" on MSDN.  
<a name="SelfValidation" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc254706761" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Self-Validation S##
Self-validation might sound as though you should be congratulating yourself on your attractiveness and wisdom, and your status as fine and upstanding citizen. However, in Enterprise Library terms, self-validation is concerned with the use of classes that contain their own validation logic.   
For example, a class that stores spare parts for aircraft might contain a function that checks if the part ID matches a specific format containing letters and numbers. You add the **HasSelfValidation** attribute to the class, add the **SelfValidation** attribute to any validation functions it contains, and optionally add attributes for the built-in Validation block validators to any relevant properties. Then you can validate an instance of the class using the Validation block. The block will execute the self-validation method.   
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>Self-validation cannot be used with the UI validation integration features for Windows Forms, WPF, or ASP.NET.  </td></tr></table><p /></div>Self-validation is typically used where the validation rule you want to apply involves values from different parts of your class or values that are not publicly exposed by the class, or when the validation scenario requires complex rules that even a combination of composed validators cannot achieve. For example, you may want to check if the sum of the number of products on order and the number already in stock is less than a certain value before allowing a user to order more. The following extract from one of the examples you'll see later in this chapter shows how self-validation can be used in this case.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[HasSelfValidation]
public class AnnotatedProduct : IProduct
  ...
<i>  ... code to implement constructor and properties goes here</i>
  ...

  [SelfValidation]
  public void Validate(ValidationResults results)
  {
    string msg = string.Empty;
    if (InStock + OnOrder &gt; 100)
    {
      msg = "Total inventory (in stock and on order) cannot exceed 100 items.";
      results.AddResult(new ValidationResult(msg, this, "ProductSelfValidation",
                        "", null));
  }
}</pre></td></tr>
            </table>
          </span>
        </div>
      The Validation block calls the self-validation method when you validate this class instance, passing to it a reference to the collection of **ValidationResults** that it is populating with any validation errors found. The code above simply adds one or more new **ValidationResult** instances to the collection if the self-validation method detects an invalid condition. The parameters of the **ValidationResult** constructor are:  
+ The validation error message to display to the user or write to a log. The **ValidationResult **class exposes this as the **Message** property.
+ A reference to the class instance where the validation error was discovered (usually the current instance). The **ValidationResult **class exposes this as the **Target** property.
+ A string value that describes the location of the error (usually the name of the class member, or some other value that helps locate the error). The **ValidationResult **class exposes this as the **Key** property.
+ An optional string tag value that can be used to categorize or filter the results. The **ValidationResult **class exposes this as the **Tag** property.
+ A reference to the validator that performed the validation. This is not used in self-validation, though it will be populated by other validators that validate individual members of the type. The **ValidationResult **class exposes this as the **Validator** property.
<a name="_Toc254706762" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Validation Rule Sets S##
A validation rule set is a combination of all the rules with the same name, which may be located in a configuration file or other configuration source, in attributes defined within the target type, and implemented through self-validation. In other words, a rule set includes any type of validation rule that has a specified name.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>**Rule set names are case-sensitive**. The two rule sets named **MyRuleset** and **MyRuleSet** are different!</td></tr></table><p /></div>How do you apply a name to a validation rule? And what happens if you don't specify a name? In fact, the way it works is relatively simple, even though it may appear complicated when you look at configuration and attributes, and take into account how these are actually processed.   
To start with, every validation rule is a member of some rule set. If you do not specify a name, that rule is a member of the default rule set; effectively, this is the rule set whose name is an empty string. When you do specify a name for a rule, it becomes part of the rule set with that name.  
<a name="_Toc254706763" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Assigning Validation Rules to Rule Sets ###
You specify rule set names in a variety of ways, depending on the location and type of the rule:  
+ **In configuration**. You define a type that you want to apply rules to, and then define one or more rule sets for that type. To each rule set you add the required combination of validators, each one representing a validation rule within that rule set. You can specify one rule set for each type as the default rule set for that type. The rules within this rule set are then treated as members of the default (unnamed) rule set, as well as that named rule set. 
+ **In Validation block validator attributes** **applied to classes and their members**. Every validation attribute will accept a rule set name as a parameter. For example, you specify that a **NotNullValidator** is a member of a rule set named **MyRuleset**, like this.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[NotNullValidator(MessageTemplate = "Cannot be null", 
                  Ruleset = "MyRuleset")]</pre></td></tr>
            </table>
          </span>
        </div>
      
+ **In SelfValidation attributes** **within a class**. You add the **Ruleset** parameter to the attribute to indicate which rule set this self-validation rule belongs to. You can define multiple self-validation methods in a class, and add them to different rule sets if required.
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[SelfValidation(Ruleset = "MyRuleset")]</pre></td></tr>
            </table>
          </span>
        </div>
      
<a name="_Toc254706764" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Configuring Validation Block Rule Sets ###
The Enterprise Library configuration console makes it easy to define rule sets for specific types that you will validate. Each rule set specifies a type to which you will apply the rule set, and allows you to specify a set of validation rules. You can then apply these rules as a complete set to an instance of an object of the defined type.   
<p class="label" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><div class="caption">Figure 2 - The configuration for the examples</div></p><img src="images\1341583D9D35B9C787BD414D48E59EC2.png" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp" />  
Figure 2 shows the configuration console with the configuration used in the example application for this chapter. It defines a rule set named **MyRuleset** for the validated type (the **Product** class). **MyRuleset** is configured as the default rule set, and contains a series of validators for all of the properties of the **Product** type. These validators include two Or Composite Validators (which contain other validators) for the **DateDue** and **Description** properties, three validators that will be combined with the **And** operation for the **ID** property, and individual validators for the remaining properties.   
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>When you highlight a rule, member, or validator in the configuration console, it shows connection lines between the configured items to help you see the relationships between them. </td></tr></table><p /></div><a name="_Toc254706765" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Specifying Rule Sets When Validating ###
You can specify a rule set name when you create a type validator that will validate an instance of a type. If you use the **ValidationFactory** facade to create a type validator for a type, you can specify a rule set name as a parameter of the **CreateValidator** method. If you create an Object Validator or an Object Collection Validator programmatically by calling the constructor, you can specify a rule set name as a parameter of the constructor. We look in more detail at the options for creating validators later in this chapter.  
+ **If you specify a rule set name** when you create a validator for an object, the Validation block will apply only those validation rules that are part of the specified rule set. It will, by default, apply all rules with the specified name that it can find in configuration, attributes, and self-validation. 
+ **If you do not specify a rule set name** when you create a validator for an object, the Validation block will, by default, apply all rules that have no name (effectively, rules with an empty string as the name) that it can find in configuration, attributes, and self-validation. If you have specified one rule set in configuration as the default rule set<i> </i>for the type you are validating (by setting the **DefaultRule** property for that type to the rule set name), rules within this rule set are also treated as being members of the default (unnamed) rule set.
The one time that this default mechanism changes is if you create a validator for a type using a** **facade other than **ValidationFactory**. As you'll see later in this chapter you can use the **ConfigurationValidatorFactory**, **AttributeValidatorFactory**, or **ValidationAttributeValidatorFactory** to generate type validators. In this case, the validator will only apply rules that have the specified name and exist in the specified location.   
For example, when you use a **ConfigurationValidatorFactory** and specify the name **MyRuleset** as the rule set name when you call the **CreateValidator** method, the validator you obtain will only process rules it finds in configuration that are defined within a rule set named **MyRuleset** for the target object type. If you use an **AttributeValidatorFactory**, the validator will only apply Validation block rules located in attributes and self-validation methods of the target class that have the name **MyRuleset**.   
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Carlos)!#:</th></tr><tr><td>Configuring multiple rule sets for the same type is useful when the type you need to validate is a primitive type such as a String. A single application may have dozens of different rule sets that all target String.</td></tr></table><p /></div><a name="_Toc254706766" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# How Do I Use The Validation Block? #
In the remainder of this chapter, we'll show you in more detail how you can use the features of the Validation block you have seen in previous sections. In this section, we cover three topics that you should be familiar with when you start to use the block in your applications: preparing your application to use the block, choosing a suitable approach for validation, the options available for creating validators, accessing and displaying validation errors, and understanding how you can use template tokens in validation messages.   
<a name="_Toc254706767" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Preparing Your Application S##
To use the Validation block, you must reference the required assemblies: you can use the NuGet Package Manager to add the required assemblies and references. In the **Manage Nuget Packages** dialog in Visual Studio, search online for the **EnterpriseLibrary.Validation** package and install it.  
If you intend to use the integration features for ASP.NET, Windows Forms, WPF, or WCF, you must also install the relevant NuGet package that contains these features: **EnterpriseLibrary.Validation.Integration.AspNet**, **EnterpriseLibrary.Validation.Integration.WinForms**, **EnterpriseLibrary.Validation.Integration.WPF**,and **EnterpriseLibrary.Validation.Integration.WCF.**   
Then you can edit your code to specify the namespaces used by the Validation block and, optionally, the integration features if you need to integrate with WCF or a UI technology.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>If you are using WCF integration, you should also add a reference to the System.ServiceModel namespace.</td></tr></table><p /></div><a name="_Toc254706768" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Choosing a Validation Approach S##
Before you start to use the Validation block, you should consider how you want to perform validation. As you've seen, there are several approaches you can follow. Table 2 summarizes these, and will help you to choose one, or a combination, most suited to your requirements.  
<table xmlns:xlink="http://www.w3.org/1999/xlink"><tr><th><p>Validation approach</p></th><th><p>Advantages</p></th><th><p>Considerations</p></th></tr><tr><td><p><b>Rule sets in configuration</b></p></td><td><p>Supports the full capabilities of the Validation block validators.</p><p>Validation rules can be changed without requiring recompilation and redeployment.</p><p>Validation rules are more visible and easier to manage.</p></td><td><p>Rules are visible in configuration files unless the content is encrypted.</p><p>May be open to unauthorized alteration if not properly protected.</p><p>Type definitions and validation rule definitions are stored in different files and accessed using different tools, which can be confusing.</p></td></tr><tr><td><p><b>Validation block attributes</b></p></td><td><p>Supports the full capabilities of the Validation block validators.</p><p>Validation attributes may be defined in separate metadata classes.</p><p>Rules can be extracted from the metadata for a type by using reflection.</p></td><td><p>Requires modification of the source code of the types to validate.</p><p>Some complex rule combinations may not be possible—only a single <b>And</b> or <b>Or</b> combination is available for multiple rules.</p><p>Hides validation rules from administrators and operators.</p></td></tr><tr><td><p><b>Data annotation attributes</b></p></td><td><p>Allows you to apply validation rules defined by .NET data annotation attributes, which may be defined in separate metadata classes.</p><p>Typically used with technologies such as LINQ, for which supporting tools might be used to generate code.</p><p>Technologies such as ASP.NET Dynamic Data that use these attributes can perform partial client-side validation.</p><p>Rules can be extracted from the metadata for a type by using reflection.</p></td><td><p>Requires modification of the source code.</p><p>Does not support all of the powerful validation capabilities of the Validation block.</p></td></tr><tr><td><p><b>Self-validation</b></p></td><td><p>Allows you to create custom validation rules that may combine values of different members of the class.</p></td><td><p>Requires modification of the source code.</p><p>Hides validation rules from administrators and operators.</p><p>Rules cannot be extracted from the metadata for a type by using reflection.</p></td></tr><tr><td><p><b>Validators created programmatically</b></p></td><td><p>A simple way to validate individual values as well as entire objects.</p><p>Useful if you only need to perform small and specific validation tasks, especially on value types held in variables.  </p></td><td><p>Requires additional code and is generally more difficult to manage for complex validation scenarios.</p><p>Hides validation rules from administrators and operators.</p><p>More difficult to administer and manage.</p></td></tr></table><p class="label" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><div class="caption">Table 2 - Validation approaches</div></p><div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Carlos)!#:</th></tr><tr><td>Remember, you can use a combination of approaches if you want, but plan it carefully. If you choose to create validators programmatically, you could expose selected properties through a configuration file if you wrote custom code to load values from the configuration file.</td></tr></table><p /></div>If you decide to use attributes to define your validation rules within classes but are finding it difficult to choose between using the Validation block attributes and the Microsoft .NET data annotation attributes, you should consider using the Validation block attributes approach as this provides more powerful capabilities and supports a far wider range of validation operations. However, you should consider the data annotations approach in the following scenarios:  
+ When you are working with existing applications that already use data annotations.
+ When you require validation to take place on the client.
+ When you are building a Web application where you will use the ASP.NET Data Annotation Model Binder, or you are using ASP.NET Dynamic Data to create data-driven user interfaces.
+ When you are using a framework such as the Microsoft Entity Framework, or another object/relational mapping (O/RM) technology that auto-generates classes that include data annotations.
<a name="_Toc254706769" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Options for Creating Validators Programmatically S##
There are several ways that you can create the validators you require, whether you are creating a type validator that will validate an instance of your class using a rule set or attributes, or you are creating individual value validators:  
+ **Use the ValidationFactory facade to create validators**. This approach makes it easy to create type validators that you can use, in conjunction with rule sets, to validate multiple members of an object instance. This is generally the recommended approach. You also use this approach to create validators that use only validation attributes or data annotations within the classes you want to validate, or only rule sets defined in configuration.  
+ **Create individual validators programmatically** by calling their constructor. The constructor parameters allow you to specify most of the properties you require for the validator. You can then set additional properties, such as the **Tag** or the resource name and type if you want to use a resource file to provide the message template. You can also build combinations of validators using this approach to implement complex validation rules.
<a name="PerformValidationDisplay" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc254706770" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Performing Validation and Displaying Validation Errors S##
To initiate validation, you call the **Validate** method of your validator. There are two overloads of this method: one that creates and returns a populated **ValidationResults** instance, and one that accepts an existing** ValidationResults** instance as a parameter. The second overload allows you to perform several validation operations, and collect all of the errors in one **ValidationResults** instance.  
You can check if validation succeeded, or if any validation errors were detected, by examining the **IsValid** property of a **ValidationResults** instance, and displaying details of any validation errors that occurred. The following code shows a simple example of how you can display the most relevant details of each validation error. See the section on <a href="#SelfValidation" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">self-validation</a> earlier in this chapter for a description of the properties of each individual **ValidationResult** within the **ValidationResults**.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>// Check if the ValidationResults detected any validation errors.
if (results.IsValid)
{
  Console.WriteLine("There were no validation errors.");
}
else
{
  Console.WriteLine("The following {0} validation errors were detected:",
                     results.Count);  
  // Iterate through the collection of validation results.
  foreach (ValidationResult item in results)
  {
    // Show the target member name and current value.
    Console.WriteLine("Target:'{0}' Key:'{1}' Tag:'{2}' Message:'{3}'", 
                      item.Target, item.Key, item.Tag, item.Message);
  }
}</pre></td></tr>
            </table>
          </span>
        </div>
      Alternatively, you can extract more information about the validation result for each individual validator where an error occurred. The example application we provide demonstrates how you can do this, and you'll see more details later in this chapter.  
<a name="_Toc254706771" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Understanding Message Template Tokens S##
One specific and very useful feature of the individual validators you define in your configuration or attributes is the capability to include tokens in the message to automatically insert values of the validator's properties. This applies no matter how you create your validator—in rule sets defined in configuration, as validation attributes, or when you create validators programmatically.  
The **Message** property of a validator is actually a template, not just a simple text string that is displayable. When the block adds an individual **ValidationResult** to the **ValidationResults** instance for each validation error it detects, it parses the value of the **Message** property looking for tokens that it will replace with the value of specific properties of the validator that detected the error.   
The value injected into the placeholder tokens, and the number of tokens used, depends on the type of validator—although there are three tokens that are common to all validators. The token **{0}** will be replaced by the value of the object being validated (ensure that you escape this value before you display or use it in order to guard against injection attacks). The token **{1}** will contain the name of the member that was being validated, if available, and is equivalent to the **Key** property of the validator. The token **{2)** will contain the value of the **Tag** property of the validator.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Carlos)!#:</th></tr><tr><td>If you display or use the **Target** token, be sure to escape it to guard against injection attacks.</td></tr></table><p /></div>The remaining tokens depend the on the individual validator type. For example, in the case of the Contains Characters validator, the tokens **{3}** and **{4}** will contain the characters to check for and the **ContainsCharacters** value (**All** or **Any**). In the case of a range validator, such as the String Length validator, the tokens **{3}** to **{6}** will contain the values and bound types (**Inclusive**, **Exclusive**, or **Ignore**) for the lower and upper bounds you specify for the validator. For example, you may define a String Length validator like this:  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[StringLengthValidator(5, RangeBoundaryType.Inclusive, 20, 
       RangeBoundaryType.Inclusive,
       MessageTemplate = "{1} must be between {3} and {5} characters.")]</pre></td></tr>
            </table>
          </span>
        </div>
      If this validator is attached to a property named **Description**, and the value of this property is invalid, the **ValidationResults** instance will contain the error message **Description must be between 5 and 20 characters.**  
Other validators use tokens that are appropriate for the type of validation they perform. The documentation installed with Enterprise Library lists the tokens for each of the Validation block validators. You will also see the range of tokens used in the examples that follow.  
<a name="_Toc254706772" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Diving in With Some Simple Examples #
The remainder of this chapter shows how you can use the Validation block in a variety of situations within your applications. We provide a simple console-based example application implemented as the Microsoft Visual Studio solution named **Validation**. You can open it in Visual Studio to view the code, or run the application directly from the bin\Debug folder.  
The application uses three versions of a class that stores product information. All of these implement an interface named **IProduct**, as illustrated in Figure 3. Each has a string property that is designed to be set to a value from an enumeration called **ProductType** that defines the valid set of product type names.  
<p class="label" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><div class="caption">Figure 3 - The product classes used in the examples</div></p><img src="images\C3146F396A34536CFF42F2A01A80A441.png" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp" />  
The **Product** class is used primarily with the example that demonstrates using a configured rule set, and contains no validation attributes. The **AttributedProduct** class contains Validation block attributes, while the **AnnotatedProduct** class contains .NET Data Annotation attributes. The latter two classes also contain self-validation routines—the extent depending on the capabilities of the type of validation attributes they contain. You'll see more on this topic when we look at the use of validation attributes later in this chapter.   
The following sections of this chapter will help you understand in detail the different ways that you can use the Validation block:  
+ **Validating Objects and Collections of Objects**. This is the core topic for using the Validation block, and is likely to be the most common scenario in your applications. It shows how you can create type validators to validate instances of your custom classes, how you can dive deeper into the **ValidationResults** instance that is returned, how you can use the Object Validator, and how you can validate collections of objects.
+ **Using Validation Attributes**. This section describes how you can use attributes applied to your classes to enable validation of members of these classes. These attributes use the Validation block validators and the .NET Data Annotation attributes.
+ **Creating and Using Individual Validators**. This section shows how you can create and use the validators provided with the block to validate individual values and members of objects.
+ **WCF Service Validation Integration**. This section describes how you can use the block to validate parameters within a WCF service.
Finally, we'll round off the chapter by looking briefly at how you can integrate the Validation block with user interface technologies such as Windows Forms, WPF, and ASP.NET.  
<a name="_Toc254706773" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Validating Objects and Collections of Objects S##
The most common scenario when using the Validation block is to validate an instance of a class in your application. The Validation block uses the combination of rules defined in a rule set and validators added as attributes to test the values of members of the class, and the result of executing any self-validation methods within the class.  
The Validation block makes it easy to validate entire objects (all or a subset of its members) using a specific type validator or by using the Object validator. You can also validate all of the objects in a collection using the Object Collection validator. We will look at the Object validator and the Object Collection validator later. For the moment, we'll concentrate on creating and using a specific type validator.  
<a name="_Toc254706774" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Creating a Type Validator using the ValidationFactory ###
You can configure the static **ValidationFactory** type from the configuration in your .config file and use it to create a validator for a specific target type. This validator will validate objects using a rule set, and/or any attributes and self-validation methods the target object contains. To configure the **ValidationFactory** class, you can use the following code.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>ValidationFactory.SetDefaultConfigurationValidatorFactory(
  new SystemConfigurationSource(false));</pre></td></tr>
            </table>
          </span>
        </div>
      You can then create a validator for any type you want to validate. For example, this code creates a validator for the **Product** class and then validates an instance of that class named **myProduct**.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Validator&lt;Product&gt; pValidator = ValidationFactory.CreateValidator&lt;Product&gt;();
ValidationResults valResults = pValidator.Validate(myProduct);</pre></td></tr>
            </table>
          </span>
        </div>
      By default, the validator will use the default rule set defined for the target type (you can define multiple rule sets for a type, and specify one of these as the default for this type). If you want the validator to use a specific rule set, you specify this as the single parameter to the **CreateValidator** method, as shown here.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Validator&lt;Product&gt; productValidator 
    = ValidationFactory.CreateValidator&lt;Product&gt;("RuleSetName");
ValidationResults valResults = productValidator.Validate(myProduct);</pre></td></tr>
            </table>
          </span>
        </div>
      The example named <i>Using a Validation Rule Set to Validate an Object</i> creates an instance of the **Product** class that contains invalid values for all of the properties, and then uses the code shown above to create a type validator for this type and validate it. It then displays details of the validation errors contained in the returned **ValidationResults** instance. However, rather than using the simple technique of iterating over the** ValidationResults** instance displaying the top-level errors, it uses code to dive deeper into the results to show more information about each validation error, as you will see in the next section.   
<a name="_Toc254706775" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Delving Deeper into ValidationResults  ###
You can check if validation succeeded, or if any validation error were detected, by examining the **IsValid** property of a **ValidationResults** instance and displaying details of any validation errors that occurred. However, when you simply iterate over a **ValidationResults** instance (as we demonstrated in the section "<a href="#PerformValidationDisplay" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Performing Validation and Displaying Validation Errors</a>" earlier in this chapter), we displayed just the top-level errors. In many cases, this is all you will require. If the validation error occurs due to a validation failure in a composite (**And** or **Or**) validator, the error this approach will display is the message and details of the composite validator.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Markus)!#:</th></tr><tr><td>Typically, you can get all the detail you need by iterating over the **ValidationResults** instance.</td></tr></table><p /></div>However, sometimes you may wish to delve deeper into the contents of a **ValidationResults** instance to learn more about the errors that occurred. This is especially the case when you use nested validators inside a composite validator. The code we use in the example provides richer information about the errors. When you run the example, it displays the following results (we've removed some repeated content for clarity).  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>The following 6 validation errors were detected:
+ Target object: Product, Member: DateDue
  - Detected by: OrCompositeValidator
  - Tag value: Date Due
  - Validated value was: '23/11/2010 13:45:41'
  - Message: 'Date Due must be between today and six months time.'
  + Nested validators:
    - Detected by: NotNullValidator
    - Validated value was: '23/11/2010 13:45:41'
    - Message: 'Value can be NULL or a date.'
    - Detected by: RelativeDateTimeValidator
    - Validated value was: '23/11/2010 13:45:41'
    - Message: 'Value can be NULL or a date.'
+ Target object: Product, Member: Description
  - Detected by: OrCompositeValidator
  - Validated value was: '-'
  - Message: 'Description can be NULL or a string value.'
  + Nested validators:
    - Detected by: StringLengthValidator
    - Validated value was: '-'
    - Message: 'Description must be between 5 and 100 characters.'
    - Detected by: NotNullValidator
    - Validated value was: '-'
    - Message: 'Value can be NULL.'
...
...
+ Target object: Product, Member: ProductType
  - Detected by: EnumConversionValidator
  - Tag value: Product Type
  - Validated value was: 'FurryThings'
  - Message: 'Product Type must be a value from the 'ProductType' enumeration.'</pre></td></tr>
            </table>
          </span>
        </div>
      You can see that this shows the target object type and the name of the member of the target object that was being validated. It also shows the type of the validator that performed the operation, the **Tag** property values, and the validation error message. Notice also that the output includes the validation results from the validators nested within the two **OrCompositeValidator** validators. To achieve this, you must iterate recursively through the **ValidationResults** instance because it contains nested entries for the composite validators.   
The code we used also contains a somewhat contrived feature: to be able to show the value being validated, some examples that use this routine include the validated value at the start of the message using the **{0} **token in the form:** [{0}] <i>validation error message</i>**. The example code parses the **Message** property to extract the value and the message when it detects that this message string contains such a value. It also encodes this value for display in case it contains malicious content.  
While this may not represent a requirement in real-world application scenarios, it is useful here as it allows the example to display the invalid values that caused the validation errors and help you understand how each of the validators works. We haven’t listed the code here, but you can examine it in the example application to see how it works, and adapt it to meet your own requirements. You'll find it in the **ShowValidationResults**, **ShowValidatorDetails**, and **GetTypeNameOnly** routines located in the region named Auxiliary routines at the end of the main program file.  
<a name="_Toc254706776" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Using the Object Validator  ###
An alternative approach to validating objects is to programmatically create an Object Validator by calling its constructor. You specify the type that it will validate and, optionally, a rule set to use when performing validation. If you do not specify a rule set name, the validator will use the default rule set. When you call the **Validate** method of the Object validator, it creates a type-specific validator for the target type you specify, and you can use this to validate the object, as shown here.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Validator pValidator = new ObjectValidator(typeof(Product), "RuleSetName");
ValidationResults valResults = pValidator.Validate(myProduct);</pre></td></tr>
            </table>
          </span>
        </div>
      Alternatively, you can call the default constructor of the Object validator. In this case, it will create a type-specific validator for the type of the target instance you pass to the** Validate** method. If you do not specify a rule set name in the constructor, the validation will use the default rule set defined for the type it is validating.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Validator pValidator = new ObjectValidator(ValidationFactory.DefaultCompositeValidatorFactory, "RuleSetName");
ValidationResults valResults = pValidator.Validate(myProduct);</pre></td></tr>
            </table>
          </span>
        </div>
      The validation will take into account any applicable rule sets, and any attributes and self-validation methods found within the target object.  
<a name="_Toc254706777" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Differences Between the Object Validator and the Factory-Created Type Validators ###
While the two approaches you've just seen to creating or obtaining a validator for an object achieve the same result, there are some differences in their behavior:  
+ If you do not specify a target type when you create an Object Validator programmatically, you can use it to validate any type. When you call the **Validate** method, you specify the target instance, and the Object validator creates a type-specific validator for the type of the target instance. In contrast, the validator you obtain from a factory can only be used to validate instances of the type you specify when you obtain the validator. However, it can also be used to validate subclasses of the specified type, but it will use the rules defined for the specified target type.
+ The Object Validator will always use rules in configuration for the type of the target object, and attributes and self-validation methods within the target instance. In contrast, you can use a specific factory class type to obtain validators that only validate the target instance using one type of rule source (in other words, just configuration rule sets, or just one type of attributes). 
+ The Object Validator will acquire a type-specific validator of the appropriate type each time you call the **Validate** method, even if you use the same instance of the Object validator every time. In contrast, a validator obtained from one of the factory classes does not need to do this, and will offer improved performance.
As you can see from the flexibility and performance advantages listed above, you should generally consider using the **ValidationFactory** approach for creating validators to validate objects rather than creating individual Object Validator instances.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Markus)!#:</th></tr><tr><td>Typically, you should use the factory-based approach when you use the block.</td></tr></table><p /></div><a name="_Toc254706778" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Validating Collections of Objects ###
Before we leave the topic of validation of objects, it is worth looking at how you can validate collections of objects. The Object Collection validator can be used to check that every object in a collection is of the specified type, and to perform validation on every member of the collection. You can apply the Object Collection validator to a property of a class that is a collection of objects using a Validation block attribute if you wish, as shown in this example that ensures that the **ProductList** property is a collection of **Product** instances, and that every instance in the collection contains valid values.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Carlos)!#:</th></tr><tr><td>You can use the Object Collection validator to validate that all the items in a collection are of a specified type.</td></tr></table><p /></div>
        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[ObjectCollectionValidator(typeof(Product))]
public Product[] ProductList { get; }</pre></td></tr>
            </table>
          </span>
        </div>
      You can also create an Object Collection validator programmatically, and use it to validate a collection held in a variable. The example named <i>Validating a Collection of Objects</i> demonstrates this approach. It creates a **List** named **productList** that contains two instances of the **Product** class, one of which contains all valid values, and one that contains invalid values for some of its properties. Next, the code creates an Object Collection validator for the **Product** type and then calls the **Validate** method.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>// Create an Object Collection Validator for the collection type. 
Validator collValidator 
          = new ObjectCollectionValidator(typeof(Product));

// Validate all of the objects in the collection.
ValidationResults results = collValidator.Validate(productList);</pre></td></tr>
            </table>
          </span>
        </div>
      Finally, the code displays the validation errors using the same routine as in earlier examples. As the invalid Product instance contains the same values as the previous example, the result is the same. You can run the example and view the code to verify that this is the case.   
<a name="_Toc254706779" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Using Validation Attributes S##
Having seen how you can use rule sets defined in configuration, and how you can display the results of a validation process, we can move on to explore the other ways you can define validation rules in your applications. The example application contains two classes that contain validation attributes and a self-validation method. The **AttributedProduct** class contains Validation block attributes, while the **AnnotatedProduct** class contains data annotation attributes.<span class="highlight" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"> </span>  
<a name="_Toc254706780" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Using the Validation Block Attributes ###
The topic, "<a href="http://go.microsoft.com/fwlink/p/?LinkID=304215" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Using Self Validation</a><i>"</i> in the Enterprise Library Reference Documentation, demonstrates use of the Validation block attributes. The **AttributedProduct** class has a range of different Validation block attributes applied to the properties of the class, applying the same rules as the **MyRuleset** rule set defined in configuration and used in the previous examples.   
For example, the **ID** property carries attributes that add a Not Null validator, a String Length validator, and a Regular Expression validator. These validation rules are, by default, combined with an **And** operation, so all of the conditions must be satisfied if validation will succeed for the value of this property.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[NotNullValidator(MessageTemplate = "You must specify a product ID.")]
[StringLengthValidator(6, RangeBoundaryType.Inclusive, 
                       6, RangeBoundaryType.Inclusive,
                       MessageTemplate = "Product ID must be {3} characters.")]
[RegexValidator("[A-Z]{2}[0-9]{4}",
              MessageTemplate = "Product ID must be 2 letters and 4 numbers.")]
public string ID { get; set; }</pre></td></tr>
            </table>
          </span>
        </div>
      Other validation attributes used within the **AttributedProduct** class include an Enum Conversion validator that ensures that the value of the **ProductType** property is a member of the **ProductType** enumeration, shown here. Note that the token **{3}** for the String Length validator used in the previous section of code is the lower bound value, while the token **{3}** for the Enum Conversion validator is the name of the enumeration it is comparing the validated value against.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[EnumConversionValidator(typeof(ProductType),
  MessageTemplate = "Product type must be a value from the '{3}' enumeration.")]
public string ProductType { get; set; }</pre></td></tr>
            </table>
          </span>
        </div>
      

#### Combining Validation Attribute Operations ####
One other use of validation attributes worth a mention here is the application of a composite validator. By default, multiple validators defined for a member are combined using the **And** operation. If you want to combine multiple validation attributes using an **Or** operation, you must apply the **ValidatorComposition** attribute first and specify **CompositionType.Or**. The results of all validation operations defined in subsequent validation attributes are combined using the operation you specify for composition type.   
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Carlos)!#:</th></tr><tr><td>Using the validation attributes, you can either use the default **And** operation for all the validators on a member or explicitly use the **Or** operation for all the validators. You cannot have complex nested validators as you can when you define the validation rules in your configuration file.</td></tr></table><p /></div>The example class uses a **ValidatorComposition** attribute on the nullable **DateDue** property to combine a Not Null validator and a Relative DateTime validator. The top-level error message that the user will see for this property (when you do not recursively iterate through the contents of the **ValidationResults**) is the message from the **ValidatorComposition** attribute.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[ValidatorComposition(CompositionType.Or,
       MessageTemplate = "Date due must be between today and six months time.")]
[NotNullValidator(Negated = true,
       MessageTemplate = "Value can be NULL or a date.")]
[RelativeDateTimeValidator(0, DateTimeUnit.Day, 6, DateTimeUnit.Month,
       MessageTemplate = "Value can be NULL or a date.")]
public DateTime? DateDue { get; set; }</pre></td></tr>
            </table>
          </span>
        </div>
      If you want to allow null values for a member of a class, you can apply the **IgnoreNulls** attribute.  


#### Applying Self-Validation ####
Some validation rules are too complex to apply using the validators provided with the Validation block or the .NET Data Annotation validation attributes. It may be that the values you need to perform validation come from different places, such as properties, fields, and internal variables, or involve complex calculations.   
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Markus)!#:</th></tr><tr><td>Use self-validation rules to define complex rules that are impossible or difficult to define with the standard validators.</td></tr></table><p /></div>In this case, you can define self-validation rules as methods within your class (the method names are irrelevant), as described earlier in this chapter in the section "<a href="#SelfValidation" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Self-Validation</a>." We've implemented a self-validation routine in the **AttributedProduct** class in the example application. The method simply checks that the combination of the values of the **InStock**, **OnOrder**, and **DateDue** properties meets predefined rules. You can examine the code within the **AttributedProduct** class to see the implementation.  


#### Results of the Validation Operation ####
The example creates an invalid instance of the **AttributedProduct** class shown above, validates it, and then displays the results of the validation process. It creates the following output, though we have removed some of the repeated output here for clarity. You can run the example yourself to see the full results.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Created and populated a valid instance of the AttributedProduct class.
There were no validation errors.

Created and populated an invalid instance of the AttributedProduct class.
The following 7 validation errors were detected:
+ Target object: AttributedProduct, Member: ID
  - Detected by: RegexValidator
  - Validated value was: '12075'
  - Message: 'Product ID must be 2 capital letters and 4 numbers.'
...
...
+ Target object: AttributedProduct, Member: ProductType
  - Detected by: EnumConversionValidator
  - Validated value was: 'FurryThings'
  - Message: 'Product type must be a value from the 'ProductType' enumeration.'
...
...
+ Target object: AttributedProduct, Member: DateDue
  - Detected by: OrCompositeValidator
  - Validated value was: '19/08/2010 15:55:16'
  - Message: 'Date due must be between today and six months time.'
  + Nested validators:
    - Detected by: RelativeDateTimeValidator
    - Validated value was: '18/11/2010 13:36:02'
    - Message: 'Value can be NULL or a date.'
    - Detected by: NotNullValidator
    - Validated value was: '18/11/2010 13:36:02'    
+ Target object: AttributedProduct, Member: ProductSelfValidation
  - Detected by: [none]
  - Tag value:
  - Message: 'Total inventory (in stock and on order) cannot exceed 100 items.'</pre></td></tr>
            </table>
          </span>
        </div>
      Notice that the output includes the name of the type and the name of the member (property) that was validated, as well as displaying type of validator that detected the error, the current value of the member, and the message. For the **DateDue** property, the output shows the two validators nested within the Or Composite validator. Finally, it shows the result from the self-validation method. The values you see for the self-validation are those the code in the self-validation method specifically added to the **ValidationResults** instance.  


#### Validating Subclass Types  ####
While discussing validation through attributes, we should briefly touch on the factors involved when you validate a class that inherits from the type you specified when creating the validator you use to validate it. For example, if you have a class named **SaleProduct** that derives from **Product**, you can use a validator defined for the **Product** class to validate instances of the **SaleProduct** class. The **Validate** method will also apply any relevant rules defined in attributes in both the **SaleProduct** class and the **Product** base class.   
If the derived class inherits a member from the base class and does not override it, the validators for that member defined in the base class apply to the derived class. If the derived class inherits a member but overrides it, the validators defined in the base class for that member do not apply to the derived class.  


#### Validating Properties that are Objects ####
In many cases, you may have a property of your class defined as the type of another class. For example, your **OrderLine** class is likely to have a property that is a reference to an instance of the **Product** class. It's common for this property to be defined as a base type or interface type, allowing you to set it to an instance of any class that inherits or implements the type specified for the property.  
You can validate such a property using an **ObjectValidator** attribute within the class. However, by default, the validator will validate the property using rules defined for the type of the property—in this example the type **IProduct**. If you want the validation to take place based on the actual type of the object that is currently set as the value of the property, you can add the **ValidateActualType** parameter to the **ObjectValidator** attribute, as shown here.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>public class OrderLine
{
  [ObjectValidator(ValidateActualType=true)]
  public IProduct OrderProduct { get; set; }
  ...
} </pre></td></tr>
            </table>
          </span>
        </div>
      <a name="_Toc254706781" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Using Data Annotation Attributes ###
The System.ComponentModel.DataAnnotations namespace in the .NET Framework contains a series of attributes that you can add to your classes and class members to signify metadata for these classes and members. They include a range of validation attributes that you can use to apply validation rules to your classes in much the same way as you can with the Validation block attributes. For example, the following shows how you can use the Range attribute to specify that the value of the property named **OnOrder** must be between 0 and 50.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[Range(0, 50, ErrorMessage = "Quantity on order must be between 0 and 50.")]
public int OnOrder { get; set; } </pre></td></tr>
            </table>
          </span>
        </div>
      Compared to the validation attributes provided with the Validation block, there are some limitations when using the validation attributes from the DataAnnotations namespace:  
+ The range of supported validation operations is less comprehensive, though there are some new validation types available in.NET Framework 4.0 and 4.5 that extend the range. However, some validation operations such as enumeration membership checking, and relative date and time comparison are not available when using data annotation validation attributes.
+ There is no capability to use **Or** composition, as there is with the Or Composite validator in the Validation block. The only composition available with data annotation validation attributes is the **And** operation.
+ You cannot specify rule sets names, and so all rules implemented with data annotation validation attributes belong to the default rule set.
+ There is no simple built-in support for self-validation, as there is in the Validation block. 
You can, of course, include both data annotation and Validation block attributes in the same class if you wish, and implement self-validation using the Validation block mechanism in a class that contains data annotation validation attributes. The validation methods in the Validation block will process both types of attributes.  
For more information about data annotations, see "<a href="http://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">System.ComponentModel.DataAnnotations Namespace</a>" on MSDN.  


#### An Example of Using Data Annotations ####
The examples we provide for this chapter include one named <i>Using Data Annotation Attributes and Self-Validation</i>. The class named **AnnotatedProduct** contains data annotation attributes to implement the same rules as those applied by Validation block attributes in the **AttributedProduct **class (which you saw in the previous example). However, due to the limitations with data annotations, the self-validation method within the class has to do more work to achieve the same validation rules.   
The self-validation method has to check the value of the **DateDue** property to ensure it is not more than six months in the future, and that the value of the **ProductType** property is a member of the **ProductType** enumeration.  To perform the enumeration check, the self-validation method creates an instance of the Validation block Enum Conversion validator programmatically, and then calls its **DoValidate** method (which allows you to pass in all of the values required to perform the validation). The code passes to this method the value of the **ProductType** property, a reference to the current object, the name of the enumeration, and a reference to the **ValidationResults** instance being use to hold all of the validation errors.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>var enumConverterValidator = new EnumConversionValidator(typeof(ProductType),
                 "Product type must be a value from the '{3}' enumeration.");
enumConverterValidator.DoValidate(ProductType, this, "ProductType", results); </pre></td></tr>
            </table>
          </span>
        </div>
      The code that creates the object to validate, validates it, and then displays the results, is the same as you saw in the previous example, with the exception that it creates an invalid instance of the** AnnotatedProduct** class, rather than the **AttributedProduct** class. The result when you run this example is also similar to that of the previous example, but with a few exceptions. We've listed some of the output here.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Created and populated an invalid instance of the AnnotatedProduct class.
The following 7 validation errors were detected:
+ Target object: AnnotatedProduct, Member: ID
  - Detected by: [none]
  - Tag value:
  - Message: 'Product ID must be 6 characters.'
...
+ Target object: AnnotatedProduct, Member: ProductSelfValidation
  - Detected by: [none]
  - Tag value:
  - Message: 'Total inventory (in stock and on order) cannot exceed 100 items.'
+ Target object: AnnotatedProduct, Member: ID
  - Detected by: ValidationAttributeValidator
  - Message: 'Product ID must be 2 capital letters and 4 numbers.'
+ Target object: AnnotatedProduct, Member: InStock
  - Detected by: ValidationAttributeValidator
  - Message: 'Quantity in stock cannot be less than 0.'</pre></td></tr>
            </table>
          </span>
        </div>
      You can see that validation failures detected for data annotations contain less information than those detected for the Validation block attributes, and validation errors are shown as being detected by the **ValidationAttributeValidator** class—the base class for data annotation validation attributes.  However, where we performed additional validation using the self-validation method, there is extra information available.   
<a name="_Toc254706782" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Defining Attributes in Metadata Classes ###
In some cases, you may want to locate your validation attributes (both Validation block attributes and .NET Data Annotation validation attributes) in a file separate from the one that defines the class that you will validate. This is a common scenario when you are using tools that generate the class files, and would therefore overwrite your validation attributes. To avoid this you can locate your validation attributes in a separate file that forms a partial class along with the main class file. This approach makes use of the **MetadataType** attribute from the System.ComponentModel.DataAnnotations namespace.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Carlos)!#:</th></tr><tr><td>If the classes you want to validate are automatically generated, use partial classes to add the validation attributes so that they don’t get overwritten.</td></tr></table><p /></div>You apply the **MetadataType** attribute to your main class file, specifying the type of the class that stores the validation attributes you want to apply to your main class members. You must define this as a partial class, as shown here. The only change to the content of this class compared to the attributed versions you saw in the previous sections of this chapter is that it contains no validation attributes.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[MetadataType(typeof(ProductMetadata))]
public partial class Product
{
<i>  ... Existing members defined here, but without attributes or annotations ...</i>
}</pre></td></tr>
            </table>
          </span>
        </div>
      You then define the metadata type as a normal class, except that you declare simple properties for each of the members to which you want to apply validation attributes. The actual type of these properties is not important, and is ignored by the compiler. The accepted approach is to declare them all as type **Object**. As an example, if your **Product** class contains the **ID** and **Description** properties, you can define the metadata class for it, as shown here.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>public class ProductMetadata
{
  [Required(ErrorMessage = "ID is required.")]
  [RegularExpression("[A-Z]{2}[0-9]{4}", 
          ErrorMessage = "Product ID must be 2 capital letters and 4 numbers.")]
  public object ID;

  [StringLength(100, ErrorMessage = "Description must be less than 100 chars.")]
  public object Description;
}</pre></td></tr>
            </table>
          </span>
        </div>
      <a name="_Toc254706783" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Specifying the Location of Validation Rules ###
When you use a validator obtained from the **ValidationFactory**, as we've done so far in the example, validation will take into account any applicable rule sets defined in configuration and in attributes and self-validation methods found within the target object. However, you can use different factory types if you want to perform validation using only rule sets defined in configuration, or using only attributes and self-validation. The specialized types of factory you can use are:  
+ **ConfigurationValidatorFactory**. This factory creates validators that only apply rules defined in a configuration file, or in a configuration source you provide. By default it looks for configuration in the default configuration file (App.config or Web.config). However, you can create an instance of a class that implements the **IConfigurationSource** interface, populate it with configuration data from another file or configuration storage media, and use this when you create this validator factory.
+ **AttributeValidatorFactory**. This factory creates validators that only apply rules defined in Validation block attributes located in the target class, and rules defined through self-validation methods.
+ **ValidationAttributeValidatorFactory**. This factory creates validators that only apply rules defined in .NET Data Annotations validation attributes.
For example, to obtain a validator for the **Product** class that validates using only attributes and self-validation methods within the target instance, and validate an instance of this class, you create an instance of the **AttributeValidatorFactory**, as shown here.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>AttributeValidatorFactory attrFactory = 
  new AttributeValidatorFactory();
Validator&lt;Product&gt; pValidator = attrFactory.CreateValidator&lt;Product&gt;();
ValidationResults valResults = pValidator.Validate(myProduct);</pre></td></tr>
            </table>
          </span>
        </div>
      <a name="_Toc254706784" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## Creating and Using Individual Validators S##
You can create an instance of any of the validators included in the Validation block directly in your code, and then call its **Validate** method to validate an object or value. For example, you can create a new Date Time Range validator and set the properties, such as the upper and lower bounds, the message, and the **Tag** property. Then you call the **Validate** method of the validator, specifying the object or value you want to validate. The example, <i>Creating and Using Validators Directly,</i> demonstrates the creation and use of some of the individual and composite validators provided with the Validation block.   
<a name="_Toc254706785" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Validating Strings for Contained Characters ###
The example code first creates a **ContainsCharactersValidator** that specifies that the validated value must contain the characters c, a, and t, and that it must contain all of these characters (you can, if you wish, specify that it must only contain **Any** of the characters). The code also sets the **Tag** property to a user-defined string that helps to identify the validator in the list of errors. The overload of the **Validate** method used here returns a new **ValidationResults** instance containing a **ValidationResult** instance for each validation error that occurred.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>// Create a Contains Characters Validator and use it to validate a string.
Validator charsValidator = new ContainsCharactersValidator("cat", 
                         ContainsCharacters.All, 
                         " Value must contain {4} of the characters '{3}'.");
charsValidator.Tag = "Validating the String value 'disconnected'";
ValidationResults valResults = charsValidator.Validate("disconnected");</pre></td></tr>
            </table>
          </span>
        </div>
      <a name="_Toc254706786" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Validating Integers within a Domain ###
Next, the example code creates a new **DomainValidator** for integer values, specifying an error message and an array of acceptable values. Then it can be used to validate an integer, with a reference to the existing **ValidationResults** instance passed to the **Validate** method this time.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>// Create a Domain Validator and use it to validate an Integer value.
Validator integerValidator = new DomainValidator&lt;int&gt;(
                                 "Value must be in the list 1, 3, 7, 11, 13.", 
                                 new int[] {1, 3, 7, 11, 13});
integerValidator.Tag = "Validating the Integer value '42'";
integerValidator.Validate(42, valResults);</pre></td></tr>
            </table>
          </span>
        </div>
      <a name="_Toc254706787" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Validating with a Composite Validator ###
To show how you can create composite validators, the next section of the example creates an array containing two validators: a **NotNullValidator** and a **StringLengthValidator**. The first parameter of the **NotNullValidator** sets the **Negated** property. In this example, we set it to true so that the validator will allow null values. The **StringLengthValidator** specifies that the string it validates must be exactly five characters long. Notice that range validators such as the **StringLengthValidator** have properties that specify not only the upper and lower bound values, but also whether these values are included in the valid result set (**RangeBoundaryType.Inclusive**) or excluded (**RangeBoundaryType.Exclusive**). If you do not want to specify a value for the upper or lower bound of a range validator, you must set the corresponding property to** RangeBoundaryType.Ignore**.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Validator[] valArray = new Validator[] 
{
  new NotNullValidator(true, "Value can be NULL."),
  new StringLengthValidator(5, RangeBoundaryType.Inclusive, 
                            5, RangeBoundaryType.Inclusive, 
                            "Must be between {3} ({4}) and {5} ({6}) chars.")
};</pre></td></tr>
            </table>
          </span>
        </div>
      Having created an array of validators, we can now use this to create a composite validator. There are two composite validators, the **AndCompositeValidator** and the **OrCompositeValidator**. You can combine these as well to create any nested hierarchy of validators you require, with each combination returning a valid result if all (with the **AndCompositeValidator**) or any (with the **OrCompositeValidator**) of the validators it contains are valid. The example creates an **OrCompositeValidator**, which will return true (valid) if the validated string is either null or contains exactly five characters. Then it validates a null value and an invalid string, passing into the **Validate** method the existing **ValidationResults** instance.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>Validator orValidator = new OrCompositeValidator(
                            "Value can be NULL or a string of 5 characters.",
                            valArray);

// Validate two values with the Or Composite Validator.
orValidator.Validate(null, valResults);
orValidator.Validate("MoreThan5Chars", valResults);</pre></td></tr>
            </table>
          </span>
        </div>
      <a name="_Toc254706788" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Validating Single Members of an Object ###
The Validation block contains three validators you can use to validate individual members of a class directly, instead of validating the entire type using attributes or rule sets. Although you may not use this approach very often, you might find it to be useful in some scenarios. The Field Value validator can be used to validate the value of a field of a type. The Method Return Value validator can be used to validate the return value of a method of a type. Finally, the Property Value validator can be used to validate the value of a property of a type.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Markus)!#:</th></tr><tr><td>Typically, you want to validate a complete instance, but if you want to validate just a single method or property, you can use the Method Return Value or the Property Value validators.</td></tr></table><p /></div>The example shows how you can use a Property Value validator. The code creates an instance of the **Product** class that has an invalid value for the ID property, and then creates an instance of the **PropertyValueValidator** class, specifying the type to validate and the name of the target property. This second parameter of the constructor is the validator to use to validate the property value—in this example a Regular Expression validator. Then the code can initiate validation by calling the **Validate** method, passing in the existing **ValidationResults** instance, as shown here.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>IProduct productWithID = new Product();
PopulateInvalidProduct(productWithID);
Validator propValidator = new PropertyValueValidator&lt;Product&gt;("ID",
   new RegexValidator("[A-Z]{2}[0-9]{4}", 
                      "Product ID must be 2 capital letters and 4 numbers.")
);
propValidator.Validate(productWithID, valResults);</pre></td></tr>
            </table>
          </span>
        </div>
      If required, you can create a composite validator containing a combination of validators, and specify this composite validator in the second parameter. A similar technique can be used with the Field Value validator and Method Return Value validator.  
After performing all of the validation operations, the example displays the results by iterating through the **ValidationResults** instance that contains the results for all of the preceding validation operations. It uses the same **ShowValidationResults** routine we described earlier in this chapter. This is the result:  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>The following 4 validation errors were detected:
+ Target object: disconnected, Member:
  - Detected by: ContainsCharactersValidator
  - Tag value: Validating the String value 'disconnected'
  - Message: 'Value must contain All of the characters 'cat'.'
+ Target object: 42, Member:
  - Detected by: DomainValidator`1[System.Int32]
  - Tag value: Validating the Integer value '42'
  - Message: 'Value must be in the list 1, 3, 7, 11, 13.'
+ Target object: MoreThan5Chars, Member:
  - Detected by: OrCompositeValidator
  - Message: 'Value can be NULL or a string of 5 characters.'
  + Nested validators:
    - Detected by: NotNullValidator
    - Message: 'Value can be NULL.'
    - Detected by: StringLengthValidator
    - Message: 'Value must be between 5 (Inclusive) and 5 (Inclusive) chars.'
+ Target object: Product, Member: ID
  - Detected by: RegexValidator
  - Message: 'Product ID must be 2 capital letters and 4 numbers.'</pre></td></tr>
            </table>
          </span>
        </div>
      You can see how the message template tokens create the content of the messages that are displayed, and the results of the nested validators we defined for the Or Composite validator. If you want to experiment with individual validators, you can modify and extend this example routine to use other validators and combinations of validators.  
<a name="_Toc254706789" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## WCF Service Validation Integration S##
This section of the chapter demonstrates how you can integrate your validation requirements for WCF services with the Validation block. The Validation block allows you to add validation attributes to the parameters of methods defined in your WCF service contract, and have the values of these automatically validated each time the method is invoked by a client.   
To use WCF integration, you edit your service contract, edit the WCF configuration to add the Validation block and behaviors, and then handle errors that arise due to validation failures. In addition to the other assemblies required by Enterprise Library and the Validation block, you must add the assembly named Microsoft.Practices.EnterpriseLibrary.Validation.Integration.WCF to your application and reference them all in your service project.   
The example, <i>Validating Parameters in a WCF Service</i>, demonstrates validation in a simple WCF service. It uses a service named **ProductService** (defined in the **ExampleService** project of the solution). This service contains a method named **AddNewProduct** that accepts a set of values for a product, and adds this product to its internal list of products.  
<a name="_Toc254706790" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Defining Validation in the Service Contract ###
The service contract, shown below, carries the **ValidationBehavior** attribute, and each service method defines a fault contract of type **ValidationFault**.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>[ServiceContract]
[ValidationBehavior]
public interface IProductService
{
  [OperationContract]
  [FaultContract(typeof(ValidationFault))]
  bool AddNewProduct(
    [NotNullValidator(MessageTemplate = "Must specify a product ID.")]
    [StringLengthValidator(6, RangeBoundaryType.Inclusive, 
        6, RangeBoundaryType.Inclusive,
        MessageTemplate = "Product ID must be {3} characters.")]
    [RegexValidator("[A-Z]{2}[0-9]{4}",
        MessageTemplate = "Product ID must be 2 letters and 4 numbers.")]
    string id,
    ...
    [IgnoreNulls(MessageTemplate = "Description can be NULL or a string value.")]
    [StringLengthValidator(5, RangeBoundaryType.Inclusive, 
        100, RangeBoundaryType.Inclusive,
        MessageTemplate = "Description must be between {3} and {5} characters.")]
    string description,
    [EnumConversionValidator(typeof(ProductType),
        MessageTemplate = "Must be a value from the '{3}' enumeration.")]
    string prodType,
    ...
    [ValidatorComposition(CompositionType.Or,
        MessageTemplate = "Date must be between today and six months time.")]
    [NotNullValidator(Negated = true,
        MessageTemplate = "Value can be NULL or a date.")]
    [RelativeDateTimeValidator(0, DateTimeUnit.Day, 6, DateTimeUnit.Month,
        MessageTemplate = "Value can be NULL or a date.")]
    DateTime? dateDue);
}</pre></td></tr>
            </table>
          </span>
        </div>
      You can see that the service contract defines a method named **AddNewProduct** that takes as parameters the value for each property of the **Product** class we've used throughout the examples. Although the previous listing omits some attributes to limit duplication and make it easier to see the structure of the contract, the rules applied in the example service we provide are the same as you saw in earlier examples of validating a **Product** instance. The method implementation within the WCF service is simple—it just uses the values provided to create a new **Product** and adds it to a generic **List**.  
<a name="_Toc254706791" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Editing the Service Configuration ###
After you define the service and its validation rules, you must edit the service configuration to force validation to occur. The first step is to specify the Validation block as a behavior extension. You will need to provide the appropriate version information for the assembly, which you can obtain from the configuration file generated by the configuration tool for the client application, or from the source code of the example, depending on whether you are using the assemblies provided with Enterprise Library or assemblies you have compiled yourself.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>XML</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>&lt;extensions&gt;
  &lt;behaviorExtensions&gt;
    &lt;add name="validation"
             type="Microsoft.Practices...WCF.ValidationElement,
                   Microsoft.Practices...WCF" /&gt;
  &lt;/behaviorExtensions&gt;

  ... other existing behavior extensions here ...

&lt;/extensions&gt;</pre></td></tr>
            </table>
          </span>
        </div>
      Next, you edit the **&lt;behaviors&gt;** section of the configuration to define the validation behavior you want to apply.  As well as turning on validation here, you can specify a rule set name (as shown) if you want to perform validation using only a subset of the rules defined in the service. Validation will then only include rules defined in validation attributes that contain the appropriate **Ruleset** parameter (the configuration for the example application does not specify a rule set name here).  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>XML</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>&lt;behaviors&gt;
  &lt;endpointBehaviors&gt;
    &lt;behavior name="ValidationBehavior"&gt;
      &lt;validation enabled="true" ruleset="MyRuleset" /&gt;
    &lt;/behavior&gt;
  &lt;/endpointBehaviors&gt;     

  ... other existing behaviors here ...

&lt;/behaviors&gt;</pre></td></tr>
            </table>
          </span>
        </div>
      <div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>Note that you cannot use a configuration rule set with a WCF service—all validation rules must be in attributes. </td></tr></table><p /></div>Finally, you edit the **&lt;services&gt;** section of the configuration to link the **ValidationBehavior** defined above to the service your WCF application exposes. You do this by adding the **behaviorConfiguration** attribute to the service element for your service, as shown here.    

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>XML</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>&lt;services&gt;
  &lt;service behaviorConfiguration="ExampleService.ProductServiceBehavior"
           name="ExampleService.ProductService"&gt;
    &lt;endpoint address="" behaviorConfiguration="ValidationBehavior"
              binding="wsHttpBinding" contract="ExampleService.IProductService"&gt;
      &lt;identity&gt;
        &lt;dns value="localhost" /&gt;
      &lt;/identity&gt;
    &lt;/endpoint&gt;
  &lt;endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange" /&gt;
  &lt;/service&gt;
  ...
&lt;/services&gt;</pre></td></tr>
            </table>
          </span>
        </div>
      <a name="_Toc254706792" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Using the Product Service and Detecting Validation Errors ###
At last you can use the WCF service you have created. The example uses a service reference added to the main project, and initializes the service using the service reference in the usual way. It then creates a new instance of a **Product** class, populates it with valid values, and calls the **AddNewProduct** method of the WCF service. Then it repeats the process, but this time by populating the product instance with invalid values. You can examine the code in the example to see this if you wish.  
However, one important issue is the way that service exceptions are handled. The example code specifically catches exceptions of type **FaultException&lt;ValidationFault&gt;**. This is the exception generated by the service, and **ValidationFault** is the type of the fault contract we specified in the service contract.   
Validation errors detected in the WCF service are returned in the **Details** property of the exception as a collection. You can simply iterate this collection to see the validation errors. However, if you want to combine them into a **ValidationResults** instance for display, especially if this is part of a multi-step process that may cause other validation errors, you must convert the collection of validation errors returned in the exception.   
The example application does this using a method named **ConvertToValidationResults**, as shown here. Notice that the validation errors returned in the **ValidationFault **do not contain information about the validator that generated the error, and so we must use a null value for this when creating each **ValidationResult** instance.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>// Convert the validation details in the exception to individual
// ValidationResult instances and add them to the collection.
ValidationResults adaptedResults = new ValidationResults();
foreach (ValidationDetail result in results)
{
  adaptedResults.AddResult(new ValidationResult(result.Message, target,
                                                result.Key, result.Tag, null));
}
return adaptedResults;</pre></td></tr>
            </table>
          </span>
        </div>
      When you execute this example, you will see a message indicating the service being started—this may take a while the first time, and may even time out so that you need to try again. Then the output shows the result of validating the valid **Product** instance (which succeeds) and the result of validating the invalid instance (which produces the now familiar list of validation errors shown here).  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th></th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>The following 6 validation errors were detected:
+ Target object: Product, Member:
  - Detected by: [none]
  - Tag value: id
  - Message: 'Product ID must be two capital letters and four numbers.'
...
+ Target object: Product, Member:
  - Detected by: [none]
  - Tag value: description
  - Message: 'Description can be NULL or a string value.'
+ Target object: Product, Member:
  - Detected by: [none]
  - Tag value: prodType
  - Message: 'Product type must be a value from the 'ProductType' enumeration.'
...
+ Target object: Product, Member:
  - Detected by: [none]
  - Tag value: dateDue
  - Message: 'Date due must be between today and six months time.'</pre></td></tr>
            </table>
          </span>
        </div>
      Again, we've omitted some of the duplication so that you can more easily see the result. Notice that there is no value available for the name of the member being validated or the validator that was used. This is a form of exception shielding that prevents external clients from gaining information about the internal workings of the service. However, the **Tag** value returns the name of the parameter that failed validation (the parameter names are exposed by the service), allowing you to see which of the values you sent to the service actually failed validation.  
<a name="_Toc254706793" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

## User Interface Validation Integration S##
The Validation block contains integration components that make it easy to use the Validation block mechanism and rules to validate user input within the user interface of ASP.NET, Windows Forms, and WPF applications. While these technologies do include facilities to perform validation, this validation is generally based on individual controls and values.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />#!SPOKENBY(Markus)!#:</th></tr><tr><td>You can use the Validation block to validate entire objects instead of individual controls. This makes it easier to perform validation consistently and to use more complex validation rules.</td></tr></table><p /></div>When you integrate the Validation block with your applications, you can validate entire objects, and collections of objects, using sets of rules you define. You can also apply complex validation using the wide range of validators included with the Validation block. This allows you to centrally define a single set of validation rules, and apply them in more than one layer and when using different UI technologies.  
<div class="alert" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp"><table width="100%" cellspacing="0" cellpadding="0"><tr><th align="left"><img class="note" src="../local/note.gif" />Note:</th></tr><tr><td>The UI integration technologies provided with the Validation block do not instantiate the classes that contain the validation rules. This means that you cannot use self-validation with these technologies.</td></tr></table><p /></div><a name="_Toc254706794" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### ASP.NET User Interface Validation ###
The Validation block includes the **PropertyProxyValidator** class that derives from the ASP.NET **BaseValidator** control, and can therefore take part in the standard ASP.NET validation cycle. It acts as a wrapper that links an ASP.NET control on your Web page to a rule set defined in your application through configuration, attributes, and self-validation.  
To use the **PropertyProxyValidator**, you must install the NuGet package named  **EnterpriseLibrary.Validation.Integration.AspNet**. You must also include a **Register** directive in your Web pages to specify this assembly and the prefix for the element that will insert the **PropertyProxyValidator** into your page.  

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="CSharp">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>C#</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>&lt;% @Register TagPrefix="EntLibValidators" Assembly="Microsoft.Practices.EnterpriseLibrary.Validation.Integration.AspNet"    Namespace="Microsoft.Practices.EnterpriseLibrary.Validation.Integration.AspNet"
%&gt; </pre></td></tr>
            </table>
          </span>
        </div>
      Then you can define the validation controls in your page. The following shows an example that validates a text box that accepts a value for the **FirstName** property of a **Customer** class, and validates it using the rule set named **RuleSetA**.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>XML</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>&lt;EntLibValidators:PropertyProxyValidator id="firstNameValidator"
   runat="server" ControlToValidate="firstNameTextBox"
   PropertyName="FirstName" RulesetName="RuleSetA"
   SourceTypeName="ValidationQuickStart.BusinessEntities.Customer" /&gt;</pre></td></tr>
            </table>
          </span>
        </div>
      One point to be aware of is that, unlike the ASP.NET validation controls, the Validation block **PropertyProxyValidator** control does not perform client-side validation. However, it does integrate with the server-based code and will display validation error messages in the page in the same way as the ASP.NET validation controls.  
For more information about ASP.NET integration, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>.  
<a name="_Toc254706795" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### Windows Forms User Interface Validation ###
The Validation block includes the **ValidationProvider** component that extends Windows Forms controls to provide validation using a rule set defined in your application through configuration, attributes, and self-validation. You can handle the **Validating** event to perform validation, or invoke validation by calling the **PerformValidation** method of the control. You can also specify an **ErrorProvider** that will receive formatted validation error messages.  
To use the **ValidationProvider**, you add the assembly named Microsoft.Practices.EnterpriseLibrary.Validation.Integration.WinForms to your application, and reference it in your project.  
For more information about Windows Forms integration, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>.  
<a name="_Toc254706796" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

### WPF User Interface Validation ###
The Validation block includes the **ValidatorRule** component that you can use in the binding of a WPF control to provide validation using a rule set defined in your application through configuration, attributes, and self-validation. To use the **ValidatorRule**, you add the assembly named Microsoft.Practices.EnterpriseLibrary.Validation.Integration.WPF to your application, and reference it in your project.  
As an example, you can add a validation rule directly to a control, as shown here.   

        <div class="code" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">
          <span codeLanguage="other">
            <table width="100%" cellspacing="0" cellpadding="0">
              <tr>
                <th>XML</th>
                <th>
                  <span class="copyCode" onclick="CopyCode(this)" onkeypress="CopyCode_CheckKey(this)" onmouseover="ChangeCopyCodeIcon(this)" onfocusin="ChangeCopyCodeIcon(this)" onmouseout="ChangeCopyCodeIcon(this)" onfocusout="ChangeCopyCodeIcon(this)" tabindex="0">
                    <img class="copyCodeImage" name="ccImage" align="absmiddle" src="../local/copycode.gif" />Copy Code</span>
                </th>
              </tr>
              <tr><td colspan="2"><pre>&lt;TextBox x:Name="TextBox1"&gt;
  &lt;TextBox.Text&gt;
    &lt;Binding Path="ValidatedStringProperty" UpdateSourceTrigger="PropertyChanged"&gt;
      &lt;Binding.ValidationRules&gt;
        &lt;vab:ValidatorRule SourceType="{x:Type test:ValidatedObject}" 
                           SourcePropertyName="ValidatedStringProperty"/&gt;
      &lt;/Binding.ValidationRules&gt;
    &lt;/Binding&gt;
  &lt;/TextBox.Text&gt;
&lt;/TextBox&gt;</pre></td></tr>
            </table>
          </span>
        </div>
      You can also specify a rule set using the **RulesetName** property, and use the **ValidationSpecificationSource** property to refine the way that the block creates the validator for the property.  
For more information about WPF integration, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>.  
<a name="_Toc254706797" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Creating Custom Validators #
While the wide range of validators included with the Validation block should satisfy most requirements, you can easily create your own custom validators and integrate them with the block. This may be useful if you have some specific and repetitive validation task that you need to carry out, and which is more easily accomplished using custom code.  
The easiest way to create a custom validator is to create a class that inherits from one of the abstract base classes provided with the Validation block. Depending on the type of validation you need to perform, you may choose to inherit from base types such as the **ValueValidator** or **MemberAccessValidator** classes, the **Validator&lt;T&gt;** base class (for a strongly typed validator) or from the **Validator** class (for a loosely typed validator).   
You can also create your own custom validation attributes that will apply custom validators you create. The base class, **ValidatorAttribute,** provides a good starting point for this.  
For more information on extending Enterprise Library and creating custom providers, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>.  
<a name="_Toc229824772" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a><a name="_Toc254706798" href="#" xmlns:xlink="http://www.w3.org/1999/xlink"><span /></a>

# Summary #
In this chapter we have explored the Enterprise Library Validation block and shown you how easy it is to decouple your validation code from your main application code. The Validation block allows you to define validation rules and rule sets; and apply them to objects, method parameters, properties, and fields of objects you use in your application. You can define these rules using configuration, attributes, or even using custom code and self-validation within your classes.  
Validation is a vital crosscutting concern, and should occur at the perimeter of your application, at trust boundaries, and (in most cases) between layers and distributed components. Robust validation can help to protect your applications and services from malicious users and dangerous input (including SQL injection attacks); ensure that it processes only valid data and enforces business rules; and improve responsiveness.   
The ability to centralize your validation mechanism and the ability to define rules through configuration also make it easy to deploy and manage applications. Administrators can update the rules when required without requiring recompilation, additional testing, and redeployment of the application. Alternatively, you can define rules, validation mechanisms, and parameters within your code if this is a more appropriate solution for your own requirements.   


# More Information #
All links in this book are accessible from the book's online bibliography on MSDN at <a href="http://aka.ms/el6biblio" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">http://aka.ms/el6biblio</a>.  
For more details on each validator, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>.  
For more information on using interception, see "<a href="http://msdn.microsoft.com/en-us/library/dn178466(v=pandp.30).aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Chapter 5 - Interception using Unity</a>" in the Developer's Guide to Dependency Injection Using Unity.  
For more information, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>, and the topic "<a href="http://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">System.ComponentModel.DataAnnotations Namespace</a>" on MSDN.  
The topic, "<a href="http://go.microsoft.com/fwlink/p/?LinkID=304215" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Using Self Validation</a><i>"</i> in the Enterprise Library Reference Documentation, demonstrates use of the Validation block attributes.  
For more information about data annotations, see "<a href="http://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">System.ComponentModel.DataAnnotations Namespace</a>" on MSDN.  
For more information about ASP.NET integration, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>.  
For more information about Windows Forms integration, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>.  
For more information about WPF integration, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>.  
For more information on extending Enterprise Library and creating custom providers, see the <a href="http://www.msdn.com/entlib" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a>.  
**General Links:**  
+ There are resources to help if you're getting started with Enterprise Library, and there's help for existing users as well (such as the breaking changes and migration information for previous versions) available on the Enterprise Library Community Site at <a href="http://www.codeplex.com/entlib/" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">http://www.codeplex.com/entlib/</a>. 
+ For more details about the Enterprise Library Application Blocks, see the <a href="http://go.microsoft.com/fwlink/p/?LinkID=290901" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library Reference Documentation</a> and the <a href="http://msdn.microsoft.com/en-us/library/dn170426(v=pandp.60).aspx" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">Enterprise Library 6 Class Library</a>.
+ You can download and work through the Hands-On Labs for Enterprise Library, which are available at <a href="http://aka.ms/el6hols" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">http://aka.ms/el6hols</a>.
+ You can download the Enterprise Library binaries, source code, Reference Implementation, or QuickStarts from the Microsoft Download Center at <a href="http://go.microsoft.com/fwlink/p/?LinkID=290898" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">http://go.microsoft.com/fwlink/p/?LinkID=290898</a>. 
+ To help you understand how you can use Enterprise Library application blocks, we provide a series of simple example applications that you can run and examine. To obtain the example applications, go to <a href="http://go.microsoft.com/fwlink/p/?LinkID=304210" xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:MSHelp="http://msdn.microsoft.com/mshelp">http://go.microsoft.com/fwlink/p/?LinkID=304210</a>. 




