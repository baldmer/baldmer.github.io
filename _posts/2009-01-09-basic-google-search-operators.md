---
layout: post
title: Basic Google search operators
tags: [Google]
---

In this post I introduce some operators with special meaning to Google. This will help researchers perform better searches.

Note: unlike other words entered as search terms, Google operators are case sensitive; be aware of this issue when you use them.

#### AND operator

This is the default operator used by Google, basically it tells Google that the terms on either side of the operator should be included in the search results.

#### OR operator

The OR operator tells Google, "Search any of the terms connected by the OR operator". For example if you only want to find a tutorial on Linux or FreeBSD, not about AIX, Solaris, OpenBSD, etc., the search query looks like this:

```
tutorial linux OR freebsd
```

#### Exclusion(-) operator

This operator is represented by the minus sign(-), it tells Google, "do not include the specified term". A typical example comes when you want to find information about biological virus, if you search the term "virus", most of the search results are related with computer virus. You would get better search results if you exclude the term "computer" from the search query.

```
virus -computer
```

#### Inclusion(+) operator

This operator is represented by the plus sign(+), it forces Google to include the indicated term on the search query. As you may know, Google ignores common words and characters, such as "how, who, where, what, I,II, III,the,and, etc." For example with the following query you would get better results:

```
linux toys +II
```

Using quoted phrase like "linux toys +II", would yield similar results. The only difference is that with quoted phrase all the terms must be linked together, this means the search would exclude any pages that refer to linux toys.

As you can see Google operators are quite easy to use. Probably average users doesn't have to worry about this, but if you are a researcher, you really need them.
