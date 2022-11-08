# Writing a good ticket

The purpose of a ticket is to communicate to a designer or developer the problem that needs to be solved.  One great way to do this is to use the story structure with a simple template like this:

> As [the actor], I want [the intent] so I can [the goal].

The three fields give the implementer understanding that they need to properly implement the feature.

* The Actor - Who is using the feature. We may also ask, "Who is prevented from using the feature?"
* The intent - Here we're describing what they are trying to acchieve.  This should not be describing any part of the UI, it's about what the user wants to do.
* The goal - What does the end state look like?  What is the big picture goal?



## Why use stories to specify a ticket?

1. These stories describe what a sufficient solution looks like.  They have "acceptance criteria" which allows the implementer to know what "done looks like". 
2. They aren't prescriptive.  The designer and developer working on the ticket are allowed to use their initiative and expertise to explore several possible solutions and pick the best.
3. It gives background information about who the actor is and what their motivation is.  This information may lead to more questions which help us develop a richer more complete understanding. 


## When is a story template not appropriate for a ticket?

1. Many times bugs are obvious and are existing in a feature that already would have had a story.  Mearly describing the bug is often sufficient.
2. Simple UI changes. (e.g. "Switch this icon to that one", "use colors from the brand guide", etc.)
