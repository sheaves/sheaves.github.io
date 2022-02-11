---
layout: post
title: Title of Post
draft_tag: 
- Tag 1
- Tag 2
---

This is a sample post. It can be viewed [here](http://sheaves.github.io/Sample-Post/){:target="_blank"}. The URL of this post depends on the part of the filename that comes after the date. If you change the filename, the URL of this post will change as well.

It's probably best to copy this file into a new post, instead of editing this directly, so that this can serve as a future reference.

## Front matter

The lines at the top of the post (between the `---`s) are the front matter. To make this post your own, change the following things:

  - **Filename**: Filenames should be of the form `YYYY-MM-DD-Title-of-Post.md`. As mentioned above, changing the filename causes the URL of the post to change.
  - **Title**: Self-explanatory
  - **Tags**: You can create your own tags, or use [existing ones](http://sheaves.github.io/topics){:target="_blank"}.

### Drafts

This post is a **draft** post, because it has `draft_tag` in the front matter instead of just `tag`. Draft posts will not appear on the [front page](http://sheaves.github.io/){:target="_blank"}, nor on the Topics or Archive page. To view the page (for previewing, say) you'll need to know the URL.

To convert a draft post to a normal post, simply remove the `draft_` from `draft_tag` in the front matter. Once that happens, the post will appear on the front page of this site, **as well as on subscribers' feeds**, so be careful! Any new tags you've created will also appear on the Topics page.

## Writing in Markdown

Most standard markdown syntax works. Here's a [cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet){:target="_blank"}.

You can also type Latex inline, like $x^2$, or in equation mode:

$$ 
\int_D d\omega = \int_{\partial D} \omega
$$

Note empty line before and after the `$$`.

For hyperlinks, I prefer to add `{:target="_blank"}` so that the link opens in a new page, but that's personal preference.

## Inserting SageMath cells

Basic SageMath code can be inserted using HTML blocks:

<div class="sage">
  <script type="text/x-sage">
G = SymmetricGroup(3)

for g in G:
    print(g)

G.structure_description()
  </script>
</div>

Note the following:

  - There should be an empty separting the block of HTML from your markdown text.
  - The SageMath/Python code should start **all the way to the left**. Standard Python indenting applies within the block.

These cells can be inserted anywhere in the post. The code in each cell runs independently from other cells - variable names won't carry over, for instance. 

### Linking SageMath cells

To write code in linked cells, use the following syntax:

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
F.<a> = NumberField(x^2 + x + 1)
F
  </script>
</div>

The only difference is that we use `div class = "linked"` instead of `div class = 'sage'`. Now the variables defined in the previous cell can be used in the next cell, and all cells that are marked `div class = "linked"`:

<div class="linked">
  <script type="text/x-sage">
G = F.galois_group()
G.structure_description()
  </script>
</div>

Linked cells have to be executed in the order that variables are defined. That's why I normally include the following note right before the first linked cell of a post:

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

There are other kinds of SageMath cells that can do different things, or that have various options enabled or disable. But this should suffice for most purposes.

Happy writing!

  


