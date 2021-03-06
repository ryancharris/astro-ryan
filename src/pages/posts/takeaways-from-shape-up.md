---
title: "Takeaways from 'Shape Up'"
publishDate: "2020-08-17"
published: true
layout: "../../layouts/BlogPost.astro"
description: "My thoughts on 'Take Shape' by Ryan Singer."
category: "books"
---

Recently, I've been having discussions with my team about our product process. They have often centered on how to move quickly while shipping useful, well thought out and thoroughly tested features.

At the suggestion of a few co-workers, I recently read [Shape Up](https://basecamp.com/shapeup) by [Ryan Singer](https://twitter.com/rjs). The book is about Ryan's (great name 🙃 ) experiences building [Basecamp](https://basecamp.com/) and the product process their team developed over time.

Generally, they broke their approach down into three sections:

1. Shaping
2. Betting
3. Building

### Shaping

Based on my reading, I would go as far as saying this is the most important part of the process. For it is here, not during development, where we need to think about problems and solutions in broad strokes.

Firstly, what pain points are our users facing? Beyond raw feedback, teams need to dig a bit further to unearth the true cause of incoming requests. **This is crucial.** We should not be reacting to requests from a singular customer without proving it's an issue for a large chunk of our user base first.

> Instead of taking the request at face value, we dug deeper.

Once a problem has been identified, the team needs to decide what their "appetite" (i.e. how much time they're willing to spend) to address a given problem might be. Doing this before ideating on solutions will help the team move through the rest of the process as it builds in a [time box](https://en.wikipedia.org/wiki/Timeboxing) for whatever work the team agrees to take on. Later, this time constraint will help teams prioritize tasks and make decisions as they approach the end of the Bulding phase.

With the problem defined and a time-box established, the team is now prepared to think of solutions -- done in broad strokes so as not to commit to any designs, flows, etc. This is where **one of my biggest takeaways from the book comes in**: while it is important to define what work will be done over the course of a project, it is equally (if not more) important to define **what work will not be done**.

### Betting

Following the shaping process, folks should create pitches for the various problems/solutions the team wants to solve. These are then brought to the "betting table", where they're discussed at a high-level and given a 👍 or 👎.

> We need to stay high level, but add a little more concreteness... People who read the pitch and look at the drawings without much context need to “get” the idea.

While this is not a new idea, the part that really struck me about the Betting phase was that the team **does not rely on a ticket backlog** to drive their decisions. Instead, the process is setup so that important issues continue to surface through customer feedback, make their way through shaping and, ultimately, end up on the betting table. Then, leadership is able to decide what work they want to undertake by looking at project ideas side by side.

This strategy helps alleviate the reactivity to customer feedback I mentioned in the section above. If a customer issue makes it to the betting table, but continues to be passed over for other work we should ask ourselves: **how important can this actually be if we keep punting on it**? Most teams would ticket these items in their backlog, but what is the point in doing that if we know we're never going to actually address all the items we put there?

> All software has bugs. The question is: how severe are they? If we’re in a real crisis—data is being lost, the app is grinding to a halt, or a huge swath of customers are seeing the wrong thing—then we’ll drop everything to fix it. But crises are rare.

Theoretically, I like the idea of not having a backlog since an ever-growing, massive wall of backlog items can cause teams stress. However, I do think it's important to keep track of ideas/issues related to your product -- though I will say, I do not believe a ticketing system is where these things should live.

### Building

At Basecamp, teams work on six-week cycles. This includes phases for design, engineering and testing (marketing announcements and documentation updates can be done afterwards) with the expectation that the team ships the new feature at the end of the cycle.

> Six weeks is long enough to finish something meaningful and still short enough to see the end from the beginning.

The main idea here is to empower the team to make their own decisions. Since they are doing the implementation, they are the ones best suited to adjust scope and priority throughout the process in order to make their delivery date.

> We expect our teams to actively make trade-offs and question the scope instead of cramming and pushing to finish tasks. We create our own work for ourselves. We should question any new work that comes up before we accept it as necessary.

**A great strategy Singer** suggests here is to carve features up into vertically deliverable slices -- i.e. finish one piece of the functionality from front to back without worrying about how it looks (for now). This has a few benefits:

- Teams stay motivated as they can see the feature starting to take shape.
- Engineers can start to uncover unforeseen issues or complications earlier on in the process.
- As slices are finished, the remaining work can continue to be broken into deliverable chunks.

As the cycle comes to an end, the team will have to decide when to stop. Since they've continued defining and re-defining the scope of the work that remains to be done, this shouldn't be too hard. Ultimately, this is more about the customer than it is about product team. If a user's experience has improved beyond its current state, then we can count that as a win because **we will never ship something perfect.**

Now you may ask: what about any of the work that remains undone? That's the beauty of this process -- instead of extending the development cycle, this work goes back into the shaping project and, eventually, makes its way to the betting table. If the work is truly important to the product, we should rely on the shaping process and that the work will make its way through the betting process once again.

### Final thoughts

I have worked at a number of companies, all of whom had their own product process. What Singer lays out in "Shape Up" is an amalgamation of the disparate processes I've seen work best over the past few years organized into a coherent process.

The big key here for me is: **the focus of our work is all about the user**. We as designers, engineers, product managers, etc. may have our own ideas or conceptions about our product, but what matters most is how our users experience it.

In my mind, using Singer's process re-focuses the team on delivering value to customers and cuts out a lot of the overhead involved with maintaining large backlogs. Does every bet work out? No, but the worst that happens is the team loses six weeks instead of six months. Using this process, a team could deliver about 8 new features a year, all of which have been well-defined and de-risked as much as possible.

And I'd wager a bet your customers would notice.
