---
author: jdhawley
title: Jekyll Categories Vs. Tags
additional_resources:
    - "[What's the difference between categories and tags in Jekyll? (Stack Overflow)](https://stackoverflow.com/q/8675841/17439238)"
    - "[Jekyll Tags on Github Pages](https://longqian.me/2017/02/09/github-jekyll-tag/)"
    - "[jekyll-tagging (Jekyll Plugin)](https://github.com/pattex/jekyll-tagging)"

tags: tag1 tag2
---

Do you know when to use categories and tags when building Jekyll sites? This blog post will tell you everything you need to know.

# Jekyll Categories Vs. Tags

When it comes to classifying and organizing content in Jekyll, there are two primary concepts that often come up - categories and tags. Categories and tags are both common concepts for classifying content across the web, regardless of whether or not Jekyll is involved.

Jekyll categories and tags have a lot of similarities, but there are also a few key differences. Many use cases could be accomplished with either, but some use cases will require one or the other.

## Similarities: Categories and Tags

Categories and tags are added to Jekyll posts in exactly the same way. Both use a YAML list or space-separated string, and both can include one or more entries. The examples below demonstrate the different formats available for specifying categories. To specify tags, use the same format but swap the `tag`/`tags` key for a `category`/`categories` key.

```yaml
tag: my-tag             # -> 1 tag(s) -> ["my-tag"]
tag: tag1 tag2          # -> 1 tag(s) -> ["tag1 tag2"]
tags: tag1 tag2         # -> 2 tag(s) -> ["tag1", "tag2"]
tags: [tag1, tag2]      # -> 2 tag(s) -> ["tag1", "tag2"]
```

Another similarity is that they can both be iterated through the same ways. Replacing `page.tags` with `page.categories` in the following example produces the list of categories on that page. Notice the array indexing necessary to get the tag name when iterating through all tags in the website.

{% raw %}
```markdown
# Iterate through tags for the current page
{% for tag in page.tags %}
    {{tag}}
{% endfor %}

# Iterate through tags for the entire site.
{% for tag in site.tags %}
    {{tag[0]}}
{% endfor %}
```
{% endraw %}

## Differences: Categories and Tags

There are a few crucial differences between categories and tags.

The first important difference is that categories are hierarchical, whereas tags are flat. In other words, `categories: category1 category2` is functionally different from `categories: category2 category1` and can affect the behavior of Jekyll. *Why* this can affect the behavior of Jekyll becomes more obvious with the second important difference, which is...

Categories can be included as a component of the generated URL for posts, whereas tags cannot. In fact, the default Jekyll configuration will use categories as part of the generated URL. The examples below will illustrate how these categories will affect the default URL scheme.

```yaml
# For a page with a filename of "2020-01-01-what-an-exciting-filename.md":

categories: category1 category2     # -> /category1/category2/2020/01/01/what-an-exciting-filename.html
categories: category2 category1     # -> /category2/category1/2020/01/01/what-an-exciting-filename.html
```

Notice that changing the order of the categories will change the final URL of the post in the generated website. This means that, in theory, a website could contain two posts with the same filename as long as the categories were different (or ordered differently). I wouldn't recommend trying it, however.

## Conclusion

Although categories and tags in Jekyll may look similar at first glance, they each have distinct uses and roles in Jekyll development. If you would like to see the classification labels in the URL, use categories. Similarly, if you'd like the labels to be hierarchical, use categories. For all other use cases, tags are likely the better way to go.