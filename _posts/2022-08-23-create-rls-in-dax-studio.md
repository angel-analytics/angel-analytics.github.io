---
title: "Create RLS in DAX Studio"
date: 2022-08-23 09:00:00 +0200
categories:
  - PowerBI
tags:
  - RLS
  - UX
---

Shameless clickbait title, you can't create row level security on DAX studio, but you can improve the experience using DAX Studio.

TLDR [Click Here](#TLDR)

Let me explain why I don't like the experience creating the roles in Power BI desktop. When you click on Manage roles a Pop up windows opens.
If you create simple expressions it will be good enough, but if you have to create complex expressions it will be a huge pain.

Building complex expressions requires you to see relationships or the data of the tables. The manage roles window blocks you from viewing that. You would have to close the window check your relationships/data, click on manage roles again, it adds extra time and worst of all there's not autocomplete 🤬.

<figure> 
<a href="{{ site.url }}{{ site.baseurl }}/assets/images/manage-roles-window.jpg">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/manage-roles-window.jpg" alt="">
</a>

<figcaption><b>Fig.1 - Bad window</b></figcaption>

</figure>

Here comes Dax Studio to the rescue!

A few months ago, I was tasked to create many roles, so I was dreading to make them. As the deadline came closer I start to think if there was a better way. I search on sqlbi but nothing came up, but one article got my attention [Introducing DEFINE COLUMN in DAX queries](https://www.sqlbi.com/articles/introducing-define-column-in-dax-queries/ "Introducing DEFINE COLUMN in DAX queries"). It basically lets you create a calculated column on the fly.

So I thought what if I use my expression in the newish DEFINE COLUMN statement, to my surprise the column was evaluating correctly.

---

### <a name="TLDR"></a>TLDR

Lets begin with a common request, you need to create RLS for your company using the employees hierarchy.

There are many good resources, but the one that I always go back is [Dynamic Row Level Security with Organizational Hierarchy Power BI](https://radacad.com/dynamic-row-level-security-with-organizational-hierarchy-power-bi "Dynamic Row Level Security with Organizational Hierarchy Power BI").
It leverages the Path function to create the hierarchy on a calculated column, then uses this column on the RLS expression.

Here is an example of the employees data, and the hierarchy loaded on Power BI.

<figure> 
<a href="{{ site.url }}{{ site.baseurl }}/assets/images/hierarchy-data.jpg">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/hierarchy-data.jpg" alt="">
</a>
<figcaption>
<b>Fig.2 - Hierarchy data</b>
</figcaption>
</figure>

Now It's time to use DAX Studio, lets create a calculated column that checks the hierarchy.

On the red box, I use the syntax for defining the calculated column, and named it `Check RLS`, then copy the expression from the RADACAD blog.

**Watch out!** The `USERPRINCIPALNAME` in Power BI Desktop shows the computer name, so instead of testing with the email I had to use the name column.
{: .notice--info}

<figure> 
<a href="{{ site.url }}{{ site.baseurl }}/assets/images/using-define-column.jpg">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/using-define-column.jpg" alt="">
</a>
<figcaption><b>Fig.3 - Using define column</b></figcaption>

</figure>

Finally you can see the results, it's evaluating _True_ for the employees that Kobe Graham has access. If you are happy with the results you can copy it to the <strike>bad window</strike> manage role window.

Using Dax Studio gives you, autocomplete, a friendly code editor and many more features. This method will help you debug and have less friction when developing complex expressions for row level security.
