---
title: Jekyll Collections Vs. Data Files
author: jdhawley
additional_resources:
    - "[Jekyll Collections](/2021/11/20/jekyll-collections.html)"
    - "[Jekyll Collections (Official Jekyll Docs)](https://jekyllrb.com/docs/collections/)"
    - "[Jekyll Data Files (Official Jekyll Docs)](https://jekyllrb.com/docs/datafiles/)"
    - "[Find Filter (Jekyll Official Docs)]([Jekyll find filter](https://jekyllrb.com/docs/liquid/filters/#find))"
---

As I've been getting into Jekyll, I've found myself pondering when to use collections vs. data files. Some of this has been due to reusing similar examples (e.g. authors for blog posts) for both collections and data files.

Today we're going to be drawing a distinction between the two options and providing examples for both.

## Collections Vs. Data Files

Let's start with data files. Data files are a great way to store data that will be referenced during the Jekyll build process, but doesn't contain a "body" of information to be formatted or rendered.

In contrast, collections are most useful for groups of related content that have a "body" to render.

As you likely noticed, the primary difference between the two is whether or not there is a "body" to render. Here are some examples to better illustrate the difference.

## Data File Examples

A great example for when to use a data file is when you are creating a simple directory. Say there are three authors for a specific blog and each of them has a contact email address that will be displayed alongside their name on every post they author. Adding those authors and contact emails to a data file means that updating the data file will update every blog post they've authored at once.

Say we have `_data/authors.yml` with the following content:

```yaml
- name: Jonathan
  email: jonathan@example.com
- name: Jason
  email: jason@example.com
- name: Jennifer
  email: jennifer@example.com
```

Then in our layout file for posts (e.g. `~/_layouts/post.html`), we can add the following:

{% raw %}

```markup
{% assign author = site.data.authors | find: "name", page.author %}
{% if author %}
    {{ author.name}} - {{author.email}}
{% else %}
    {{page.author}}
{% endif %}
```

{% endraw %}

This uses the [Jekyll find filter](https://jekyllrb.com/docs/liquid/filters/#find) to find the author from our data file and display the author's email address if the author is found. This could be extended to include additional data such as twitter handle, personal website, etc.

Note, however, that there is no rendered "body" of information to be displayed on a "per author" basis. For example, there is no paragraph detailing the author's expertise or their work history. If we want that kind of "body" of content for each author, then collections are going to be a better tool than a data file.

## Collection Example

To keep continuity between the examples, we're going to continue with the author use case. Let's say we're trying to display the name, contact email, and a short excerpt about the author at the bottom of any posts they've authored.

We will start by setting up a basic collection with our three authors.

First, add the following section to your `_config.yml`:

```yaml
collections:
    authors:
        output: false
```

Note that we're setting `output: false` because we don't need a page rendered for each author. If we wanted each author to have their own page with even more information, we could set `output: true` instead.

Next, we'll add some content for each of our three authors.

First, put the following content in `~/_authors/jason.md`:

```markdown
---
name: Jason
email: jason@example.com
---

Jason has been part of the team for over two years, though he has never written a single article. In fact, we can't even find a reason why Jason is still on the team.
```

Next, put the following content in `~/_authors/jennifer.md`:

```markdown
---
name: Jennifer
email: jennifer@example.com
---

Jennifer is the newest addition of our team at just over four months of tenure. She brings valuable experience from several different industries and hopes to use these insights to take our blog to the next level.
```

Lastly, put the following content in `~/_authors/jonathan.md`:

```markdown
---
name: Jonathan
email: jonathan@example.com
---

Jonathan has been authoring posts on our website for around three years now. His hobbies include long walks on the beach, poetry, and claiming superiority over all other "J names".
```

Note that the fields previously found in the data file are now found in the front matter of the collection items. There is also a "body" of content to be rendered for each author. We can now take advantage of that to display the author information at the bottom of each blog post.

In the blog post template (e.g. `~/_layouts/post.html`), insert the following right after rendering {% raw %}`{{content}}`{% endraw %}:

{% raw %}

```html
{% assign author = site.authors | find: "name", page.author %} {% if author %}
<h3>More about the author:</h3>
<p>{{author.name}} - {{author.email}}</p>
<p><i>{{author.content}}</i></p>
{% else %}
<p>Written by {{page.author}}</p>
{% endif %}
```

{% endraw %}

This will display the name, email, and biography of whoever is specified as the author of the post. The final result will end up something like this:

{% include image.html src="2021-11-24-jekyll-collections-vs-data-files/author_excerpt.png" %}
