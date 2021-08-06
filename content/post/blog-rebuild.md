+++
author = "Kevin Oliver"
title = "Welcome to Technicalpanda 2.0!"
date = "2020-10-03"
description = "New blog update overview and skills learned."
tags = [
    "Blog", "Hugo", "Github", "Static Web App", "Azure"
]
+++

# Blog renovations and skill building

In a effort to immerse myself more in development focused tools and processes, I have update my blog to run in Azure as a static web application using the [Hugo](https://gohugo.io/) web app generator. The site files are hosted in Github. As I update the site github actions will cause a update of the site to happen automatically. New content will be pushed to Azure without any interactions required by me. 

#### What is Hugo
> Hugo is a fast and modern static site generator written in Go, and designed to make website creation fun again. Hugo is a general-purpose website framework. Unlike systems that dynamically build a page with each visitor request, Hugo builds pages when you create or update your content. Since websites are viewed far more often than they are edited, Hugo is designed to provide an optimal viewing experience for your website’s end users and an ideal writing experience for website authors.<br>
> — <cite>Hugo Website[^1]</cite>
[^1]: The above quote is excerpted from [What is Hugo](https://gohugo.io/about/what-is-hugo/).


Unlike CMS software like Wordpress, new post or pages are added through a [Markdown](https://daringfireball.net/projects/markdown/) file (new-post.md). This files provide the raw format that Hugo understands and will render the page accordingly. Markdown provides an easy way to format plain text without requiring any difficutl coding to create that format. 

#### Example Hugo Markdown file
{{< highlight md >}}
+++
author = "Kevin Oliver"
title = "Blog and Skill update"
date = "2020-10-03"
description = "New blog update overview and skills learned."
tags = [
    "Blog", "Hugo", "Github", "Static Web App"
]
+++

### New Blog post
Just some normal text for general paragraphs

#### Ordered List

1. First item
2. Second item
3. Third item

#### Unordered List

* List item
* Another item
* And another item

{{< /highlight >}}


