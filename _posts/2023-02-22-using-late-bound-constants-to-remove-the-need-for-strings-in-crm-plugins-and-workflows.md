---
layout: post
title:  "Using Late-bound constants to remove the need for strings in CRM Plugins and Workflows"
date:   2023-02-22 11:00:00 +0000
categories: crm dynamics update
---

Have you seen something like this before:
```csharp
    var name = target.Attributes["abc_fullname"];
    var phone = target.Attributes["abc_phone"];
    var email = target.Attributes["emailaddress1"];
    var property = target.Attributes["abc_proprety"];
```
Did you spot the issue ? 
this code would throw an error because there's a misspelling in the "property" attribute logical name. Now imagine dealing with lots of attributes with complex names and never knowing if you have an issue until you run the code and the exception just jumps in your face.
There is a better way to handle this so we can have a cleaner code which is easier to maintain and debug.
One way would be to change these late-bound attributes and use early bound contracts, which is good but it might introduce some complexity that you want to avoid.
The easier and less complex way is to use auto generated late-bound constants.
Here's how to do that:
## Generating Late Bound Constants using XRMToolbox
Open your XRMToolbox and look for a tool called **Latebound Constants Generator**

![Late Bound Image 1](/assets/late-bound-image-1.PNG)

It will load all the entities in your connected environment.
Now, Check the ones that you need to generate constants for, and click on Generate

![Late Bound Image 2](/assets/late-bound-image-2.PNG)

Now you have the constants CS files saved.

Include them in your project and use them to replace the above code with this
```csharp
    var name = target.Attributes[Customer.FullName];
    var phone = target.Attributes[Customer.PhoneNumber];
    var email = target.Attributes[Customer.EmailPrimary];
    var property = target.Attributes[Customer.Property];
```
Looks much better, and since this code is auto generated, you can avoid misspellings and easily maintain and debug your code.

I believe using this approach would make it much easier to spot issues and to have a more clean and readable code which is better for your productivity and for those who will need to work on this code after you.

Happy Coding :)



