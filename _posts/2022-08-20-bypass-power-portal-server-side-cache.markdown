---
layout: post
title:  "Bypass Power Portal Server Side Cache When Using Fetchxml"
date:   2022-08-20 12:00:00 +0200
categories: crm powerportal powerapps
---

Power Apps Portals are very powerful, but they have their pros and cons; One of the biggest con is the server side cache between power portals and the dataverse data.
currently, When portals retrieve data from the dataverse, it caches these data for 15 minuets before it can be refreshed, this is done to help improve the portal performance, however in some scenarios, you need the data to be up to date in real time. and there's a solution for that if you retrieve data using fetchxml.

### The Problem

If you're using fetchxml to retrieve data in power portal pages, the data will only refresh every 15 minuets, so between these 15 minuets intervals, if the data in dataverse were changed by any means, for ex. workflows, power automate flows, or even manually editing them, the changes will not be reflected in your portal page until the next refresh window, which is not acceptable in some business scenarios.

### The Solution

Solving this issue would be very simple, Just add a unique value in your fetchxml that changes from one request to the other. For example, add an always true condition with a value that is always changing. like a field that you know would never equal a random number generated, or a date field that would never equal the value of "DateTime.Now" up to the second.

for example add the following code to your page:

{% raw %}
```xml
{% assign currentDate = now | date: "yyyy-MM-ddTHH:mm:ssZ" %}
{% assign conditionCache = '<condition attribute="createdon" operator="ne" value="' | append: currentDate | append: '" ></condition>' %}
```
{% endraw %}

and include the "**conditionCache**" variable in your fetchxml. since it's very unlikely that any records would be created in the same second your page loads, this condition should be irrelevant to your request and since every time the page is loaded the datetime value would be different, the fetchxml will always have a unique value in it. which would cause it to bypass the server side 15 minute cache rule and retrieve the up to date data.

and that's it, Job done.\
Happy Coding :computer: