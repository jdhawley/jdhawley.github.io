---
author: jdhawley
date: "2021-11-21"
additional_resources: 
    - "[Variables (Official Jekyll Documentation)](https://jekyllrb.com/docs/variables/)"
    - "[concat - Liquid Filter](https://shopify.dev/api/liquid/filters/array-filters#concat)"
---

As I was working on this website yesterday, I found myself looking for a way to loop through every page, post, and collection item for a specific tag on the page. This seemed like a fairly simple use case but I was having a hard time finding documentation on a solution that behaved the way I wanted it to.

Since I'm just beginning to learn Jekyll, it turned out to be more of a terminology problem. I wasn't clear on the distinctions between several Jekyll terms and it caused me unnecessary confusion. Let's talk about those terms real quick.

## Terminology

**Page** - A piece of renderable content that does not belong to a collection.

**Document** - A piece of renderable content in a collection.

**Post** - A piece of renderable content within the `posts` collection. Although the `posts` collection is a special collection, it is nonetheless still a collection. Therefore, posts are also considered documents.

See {{page.additional_resources | first}} for more details.

## Solution

After learning the distinctions between the definitions, the solutions I was seeing started to make sense. Most use cases involve looping through all pages, all posts, or all collection items, but never looping through all of them together. As such, there is no magic bullet for "all content accessible through a URL".

That doesn't mean we can't still accomplish what we set out to do. We just need to take an extra step to do it.

Looking at the Jekyll documentation, we can see there is a `site.pages` variable that we can use to access all pages (i.e. content *not* in a collection). We will also see there is a `site.documents` variable (i.e. content *in* a collection). All we need to do is combine these two arrays together and we're golden. 

Here's an example where we loop through all rendered content to find pages and documents with a specific tag.

{% raw %}
```markdown
{% assign alldocs = site.pages | concat: site.documents %}
{% for doc in alldocs %}
    {% for tag in doc.tags %}
        {% if tag == "example" %}
            <li>
                <a href="{{doc.url}}">{{doc.title}}</a>
            </li>
        {% endif %}
    {% endfor %}
{% endfor %}
```
{% endraw %}

## Conclusion

After clearing up the Jekyll terminology, this became a pretty easy problem to solve. Pages are rendered content that does not belong to a collection, and documents are rendered content that belong to a collection. Joining the two groups together will provide all of the rendered content, whether or not it belongs to a collection.