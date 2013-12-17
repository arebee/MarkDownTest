---
Source File Name: 40-Logging.docx
AssetID: 22512763-a6be-42dd-a801-61eadfdb5390
Title: Configuring Filters
Order In ToC: 1-4-1-1-5
Output Filename: 1-4-1-1-5_Configuring Filters.markdown
---

#### Markdown Test ####
# Configuring Filters #
----------

You can filter log entries based on their categories and their priorities. You can also entirely enable or disable logging or add a custom logging filter.  
It is useful to store the configuration values for these filters in a configuration file. This makes it easy to change these values dynamically, for example by changing the range of priorities.  

# Priority Filters #
You can use the **PriorityFilter** class to allow or deny log entries based on their priority. The following code sample shows a priority filter that only allows log entries with a priority between two and 99.  

```csharp
PriorityFilter priorityFilter = new PriorityFilter("Priority Filter", 2, 99);
 
LoggingConfiguration config = new LoggingConfiguration();
config.Filters.Add(priorityFilter);
```


# Category Filters #
You can use the **CategoryFilter** class to allow or deny log entries based on their categories. The following code sample shows a category filter that denies log entries with the **BlockedByFilter** category.  

```csharp
ICollection&lt;string&gt; categories = new List&lt;string&gt;();
categories.Add("BlockedByFilter");
 
CategoryFilter categoryFilter = new CategoryFilter(
  "Category Filter", categories, CategoryFilterMode.AllowAllExceptDenied);
 
LoggingConfiguration config = new LoggingConfiguration();
config.Filters.Add(categoryFilter);
```


# Log Enabled Filter #
You can use the **LogEnabledFilter** class to provide a global switch to turn logging on and off. The following code sample shows how to use this filter.  

```csharp
LogEnabledFilter logEnabledFilter = 
  new LogEnabledFilter("LogEnabled Filter", true);
 
LoggingConfiguration config = new LoggingConfiguration();
config.Filters.Add(logEnabledFilter);
```


