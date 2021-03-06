---
title: "Planning my digital garden"
publishDate: "2020-08-11"
published: true
layout: "../../layouts/BlogPost.astro"
description: "Thoughts about redesigning my website and turning it into a digital garden."
category: "garden"
---

### Preface

This site has slowly been evolving over the past few years. First, it was primarily used for job searching. Then, it became a repository for my "very polished" articles, which I published with sporadic frequency.

Now, I am in need of something that goes beyond a chronologically listed blog roll, something many smarter folks than me have begun calling a [digital garden](https://joelhooks.com/digital-garden).

The TL;DR of this metaphor is that not all content needs to be perfectly formed. It can be nascent "seeds" that you will cultivate and refine over time. Some will bloom in full, others may wither and die.

As I embark on the adventure of developing my digital green thumb, I thought it was important that I lay out my goals and expectations for this transformation (inspired by [Kurt Kemple's blog post](https://theworst.dev/redesigning-the-worst-dot-dev/)). What follows is an attempt to organize my thoughts.

### Goals

#### 1. Code as a first class citizen

It's probably not much of a surprised, but my site has code snippets strewn throughout. They're currently what I could call passable, however, they don't look so great -- especially the inline blocks. Since I will only be adding more snippets over time, I need to make these as readable and accessible as possible.

[Prince Wilson's](https://prince.dev/) site is a particular inspiration in this vein. Beyond a beautiful display, Prince's code snippets also include line highlighting! This will be super useful when explaining specific aspects of my code blocks.

#### 2. Footnotes (or callouts)

As you can see above, I add a lot of links to my posts. Whether they're reference material, inspirational or just plain cool, I'd like to improve how these look. My current thinking is I can add this information in the frontmatter of my posts and create a component that consumes and displays them.

I'm of two minds here. I could do footnotes with inline annotations (a la [Jason Lengstorf](https://lengstorf.com)) or inline callouts similar to Prince and [Monica Powell](https://www.aboutmonica.com).

#### 3. Curation mechanism

This quote from Joel struck me. Like him, I am also old enough to remember a time gone by where not every website was a chronologically ordered list of posts. Before everything was so... the same.

> I'm convinced that paginated posted sorted chronologically fuckin' sucks. What makes a garden is interesting. It's personal. Things are organized and orderly, but with a touch of chaos around the edges. ~ [Joel Hooks](https://joelhooks.com/digital-garden)

Many of the amazing people cited throughout this article have some sort of mechanism for highlighting specific content and/or grouping related things. Sure, you can still see an "All Articles" list, however, the homepage is structured in a way so as to call out specific things they're working on, interested in, etc. This way, it becomes less about chronology and more about cultivation.

#### 4. Search functionality

A strange yet fulfilling part of creating content is that you often find yourself referring back to your own posts.

Since the garden is meant for public consumption, including my own, there needs to be a way to easily find notes I've previously planted. This will also help in the "tending" of my garden as I can look at past content and decide whether it needs to be updated.

#### 5. Classified content

I'm not 100% sure what I want to do here and there are a few ways to go.

In the spirit of digital gardening, I would like to note somewhere in the UI whether a post is a seed, sapling or plant (if for no other reason than informing the reader how shaped the content is). I don't want it to deincentivize users from reading my posts -- more set expectations as to what they're about to read.
