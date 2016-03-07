---
layout: document
category: tags
published: true
title: "Comments count"
Description: The comments_count tag is used to display the number of comments associated with a particular article.
tags:
  - Comment tags
---

# Comments count

On this page:

* [Syntax](#user-content-syntax)
* [Attributes](#user-content-attributes)
* [Examples](#user-content-examples)

## Syntax

~~~ html
<txp:comments_count />
~~~

The **comments_count** tag is a *single* tag which is used to display the number of comments associated with a particular article.

Though **comments_count** can be used independently, it is also called by [comments_invite](comments-invite) to append the comments count to the **comments_invite** link.

Used in an article form.

## Attributes

This tag has no attributes.

## Examples

### Example 1: Display comment invitation and count

But only if any comments are associated with the current article.

~~~ html
<txp:if_comments>
    <p>
        <txp:comments_invite showcount="0" /> |
        <txp:comments_count /> respones received.
    </p>
</txp:if_comments>
~~~

Other tags used: [comments_invite](comments-invite), [if_comments](if-comments).