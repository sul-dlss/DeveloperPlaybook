# Technical Lead

- "being a good tech lead means some version of: good execution with a team that’s happy." - https://blog.coleadership.com/what-does-it-take-to-be-a-successful-tech-lead/
- “Success for me in this role is if I can get the team to accomplish the goals that the company is looking for.” - https://blog.coleadership.com/what-does-it-take-to-be-a-successful-tech-lead/
- "meet with stakeholders outside of the team keeping a good information flow, or to remove blockers."- https://www.patkua.com/blog/the-definition-of-a-tech-lead/
- "A Tech Lead is a software engineer responsible for leading a team and alignment of the technical direction." - https://www.patkua.com/blog/the-definition-of-a-tech-lead/
- "Lead will be the interface between the team and the management." - https://www.weetechsolution.com/blog/technical-leader-roles-and-responsibilities
- "Providing a strong technical direction involves establishing a technical vision, resolving technical disagreements and managing the technical quality of team deliverables. Effective technical leadership ensures the team uses appropriate engineering practices (such as CD or automated testing), invests in continual improvements to tooling or technical debt, and that the system evolves to meet its changing needs and environment."- https://www.patkua.com/blog/the-definition-of-a-tech-lead/

# It's About Team Achievement

"excelling in delivery and execution doesn't just mean being able to do those things yourself, but being able to coordinate, communicate, plan, and delegate effectively." - https://blog.coleadership.com/what-does-it-take-to-be-a-successful-tech-lead/

TL is a full-time job; I don't believe you can be a good TL and a developer at the same time because being TL takes at least 70% of work time, and the remaining time is usually broken up into a few minutes here and there.

## It's about OTHER people

The TL role is to help the _other_ devs be as productive as possible on sprint and work-cycle goals.  The TL is the interface to the info the team needs to work effectively.
- knock down roadblocks, or help find another way around
- make sure PRs are reviewed promptly, and with an eye to quality and coding goals (below)
- ensure team awareness of current priorities, which do shift
- keep work on track
- try to anticipate gotchas and address them

The TL may not be the expert in the tech
- use the team ... tap into expert knowledge wherever it resides

The TL may not be able to code much
- more devs --> more TL time needed coordinating
- never take blocking tickets
  - it's more important for you to be available for pairing, consulting with PO, ticket writing, board grooming, etc.
- take tiny tickets so they won't languish half done
- take unfun tickets so other devs can be happier
  - e.g. review who has access to endpoints: this can be done in small pieces, and no one will be excited to do it

_“What am I uniquely capable of doing?”_ - do these things; leave the rest on the board for others.  As TL, you are in a key position for information flow. Honor that via ticket writing, board grooming, pairing, etc.

It helps to know who you are, what your skills and talents are, and to ask for help when you need it.

## Project Management Has to Be Done

... and team members are happier if it is done well.

### It's About Process
There needs to be a transparent process that:
- documents the work cycle and sprint goals for easy lookup (e.g epics, milestones)
- enables devs to easily choose the highest priority work for the sprint and work cycle (e.g ready column)
- keeps all devs actively engaged in working towards sprint and work cycle goals (e.g. population of ready column, easy way for devs working on a ticket to surface problems and get resolution quickly)
- makes it easy for the Tech Lead (TL), the Product Owner (PO) and management to see which work is in progress and how it relates to the goals (the board as a whole)

If you're TL, and the work-cycle PO is mostly unavailable, and there's no appointed scrum master ... project management still needs to be done.  You don't have to do everything yourself ... but you need to ensure it all gets done.

There's also engineering management opportunities when you are TL:
- Which tech debt should we be paying?
- e.g. “How should CDL fit into ETD work cycle?”
- e.g. “Can the team be more-or-less single focused on ETD work for x weeks, then be single focused on Sinopia afterwards?”

Ask the questions, start the discussions, etc. that will help the work-cycle and the team be as successful and happy as possible.

### The Board the Board the Board
We've been using "project boards" for transparency of process during workcycles.  More on them below.

### Keep Track of Action Items Somewhere

Action Items may come up in a variety of contexts:
- during standup / discussions after standup
- during meetings between TL and PO, or any other subset of the team
- during full team meetings (planning, retro)
- conversations during pairing ...
- planned and impromptu meetings

For ETD 2020Q3 work cycle, many action items ultimately became tickets, but they often started out as text in the running notes document, assigned to an individual with a comment.  Sometimes the action item was "create a ticket about" because writing the ticket needed a few more minutes than were available at the time.

### Working with the PO

Each TL and each PO and each project is different.  The "right" amount of time for the TL and the PO to confer depends on individual styles, on the actual work being done for the work cycle, the active work in the moment, etc.

Keep a running list of topics to be discussed -- the running notes are a good place for this nascent "agenda" for a TL + PO meeting.

- meet as often as you need to, or if that's not possible, as often as you can.
  - for ETD 2020Q3, there were so many details and questions, the TL and the PO met for 30 min every day
  - for SDR features 2019Q3, there was so little availability of the PO, we had to ensure we utilized the planning meeting time to get PO questions answered.

I'm guessing the ideal is between those 2 extremes.  Maybe you can meet ad hoc as needed.  Maybe you can get decisions made over slack.  Maybe it works best for a particular TL + PO pairing to have n meetings a week on the calendar.

Ultimately: are the right topics appearing on your topics list and are you able to discuss them and get decisions made so work isn't blocked?

Also, the PO might need help with:
- identifying which issues / designs / pull requests / deployments need PO sign off
- tracking pull requests merged and changes deployed to production for the PO to be aware of ("we rolled out X, Y, and Z on Tuesday")
- tracking the progress in a particular sprint for reporting out (I've used github milestones to indicate the work associated with each sprint)

It behooves the TL to know what the best way to contact the PO is (e.g. slack?  tag in github issue?  email?  other?)

see also "not everyone knows our toolset" below.

### Working with the UX Designer

The designer might need help with:
- identifying which issues need designer input
- identifying which PRs need designer feedback
- knowing how and when to test coded solutions in situ (e.g. deployed to stage VM)

It behooves the TL to know what the best way to contact the designer is (e.g. slack?  tag in github issue?  email?  other?)

see also "not everyone knows our toolset" below.

### Setting up the Project Board

We like zenhub because we automatically get existing tickets from multiple projects, and can get tickets from multiple github organizations, as needed.  Sometimes a github project works well, instead.

__Zenhub__: In zenhub, when you add a repo to the board, all the tickets show up.  If there are a lot of tickets, we have found it useful to create an "out of scope" column.  If the board has an "out of scope" column before you add the github repo to the board, you may be able to get all existing issues to land there.  There is also a way to bulk move tickets to another column in zenhub (I always have to look it up).

Zenhub performance is related to how many repos are part of a board, so it is optimal if you can keep the number small.  We've had projects with lots of repos needed, but when certain work was finished, we could ditch some of those repos and improve the performance of the zenhub board.

### Not Everyone Knows Our Toolset

It may be useful to walk a PO, a UX Designer, or anyone newly interacting with the team during a work cycle, through our toolset.  It may put them at ease, and could improve efficiency as well.

- How we use git, github
  - issues
  - pull requests
    - what are they?
    - how to do a review, with an approve or reject or comment ...
  - how to review code changes in a branch
    - We can deploy a git branch to a VM for in situ vetting
    - It needs to be coordinated (it's not necessarily just sitting there waiting)
- How we use zenhub
  - what are the columns?  what do they mean?
  - how do tickets get into each column?
  - how do we sort them?
- How we use GoogleDocs for running notes and action Items
  - assigning something to a person with a comment
  - resolving the item via the comment AND with strikethrough for text
  - making the notes text a different color from the agenda text

### WorkCycle Planning

__EPICS__: It's often useful to be able to articulate the work cycle goals in a small number of achievable "epics" for the work cycle. They can be specific or a bit vague.  You're looking for a short list that can be the organizing framework for a presentation slide or an elevator speech.  You might want to be able to complete this phrase for each EPIC "... and we will know this is done when x, y, z are true."

Some examples:

- Decouple ETD app from direct access to DOR
- Top PO Priorities (to Improve Service Management) <-- note how this allows for a bunch of disparate things to be in a single Epic, but it's clear they have been vetted and prioritized
- Improve UI Accessibility <-- vague as to when it is "finished" but clear as a category of work
- Allow Multiple Moabs For a Single Druid
- Convert "Bulk Updates" (Sync) to "Bulk Actions" (ASync)
- Replace Cognito Authorization

Note that Epics can be too vague to be useful, or too lofty to be realistic. For example, "Replace Fedora3" might be too much for a single work cycle and it may not be clear what it entails.  If so, break it down.  Maybe "Decouple App1, App2 and App3 from direct access to DOR" would be an Epic in a work cycle, or "Explore Valkyrie as a Fedora3 Alternative", with a clear idea of "and we will know this is done when we have indicated which of the N desired functionalities Valkyrie can provide."

__Have Realistic Goals__
- 5 epics, max ... preferably 3 or 4.
- better to underpromise than overpromise
- what can be viewed as stretch goals?

__Prioritize__
- "what if you could only have ONE thing?" ... "TWO things" ... etc.
- stretch goals are your friends

### Sprint Planning

__Have Realistic Goals__
- better to underpromise than overpromise
- what can be viewed as stretch goals?

__Prioritize__
- "what if you could only have ONE thing?" ... "TWO things" ... etc.
- stretch goals are your friends


### Work Cycle Flow Pieces

__Before the Work Cycle__
- schedule a kickoff meeting (Vivian)
- prep inception deck slides (PO)
- have a realistic agenda for the meeting, and expected outcomes
- set up Google Drive for random related documents, if needed
- prepare a running notes doc (minimum: include a "landing page" of links and the notes from each retro)
- prepare the initial project board (TL with help from PO) (more below)
- set up a zoom room, or decide to use an existing one, for meetings
- put meetings on the calendar (with all the correct invitees!  and an appropriate room during non-COVID-19 times)
  - schedule usually decided during kickoff meeting

__Ongoing__
- create / update github issues as needs arise (TL + PO?)
- groom the board (more below)
- ensure pull requests (PRs) are reviewed quickly (TL)

__Before Each Sprint__
- ensure the TL and PO are ready for the full team sprint planning meetings
  - are the sprint goals crisp?
  - are the sprint goals clear?  are they documented somewhere (e.g. in a github milestone?)
  - are the sprint goals achievable?
  - does the board reflect the sprint goals?
- are the tickets in the ready column truly actionable?
  - try to put on a another developer's mindset and read each ticket in the ready column ... is it actionable for that developer?
- sprint planning meetings should maximize team involvement in vetting sprint goals and tickets
  - spend 10 minutes thinking about how to make the sprint planning meeting crisp and effective
  - is there a boring part of the meeting you can cut out?
  - are there conversations among a smaller group that can happen ahead of time, or at another time?
  - feel free to ask your colleagues for input
  - be clear about expectations and outcomes for the planning meeting
- set up agenda for planning meetings
- ensure notes are taken (esp for discussions and action items)

__During Sprint__
- run standup meetings (keep on track, crisp, discussions at end)
- ensure there are ALWAYS prioritized, actionable tickets in the ready column
- PR review happens promptly.  (within hours;  certainly no more than one day for review)
- PO and Designer review of work, when required, happens promptly.

__End of Sprint__
- prepare demos, if desired
- send emails out communicating progress and achievements (almost always the PO)
- pick the format/agenda for each retro meeting (could be one format for all).  Be clear about expectations and outcomes.
- run the retro meeting (keep on track, crisp, discussions at end, note action items)
  - spend 10 minutes ahead of time thinking about how to make the retro meeting crisp and effective
  - is there a boring part of the meeting you can cut out?
  - feel free to ask your colleagues for input (e.g. Josh Greben can rattle of a lot of retro format ideas)
- set up the retro agenda
- ensure notes are taken, esp for discussions and action items (but also for individual reports)


# What Does a TL _DO_?

## Pairing
### With Devs
- TL should be as available as possible to devs to answer questions or pair
  - (this is one of the reasons why the TL should not take blocking tickets or large tickets -- the TL priority is to keep the other devs unblocked)
- If you think a dev took a ticket but might unknowingly be missing some context or understanding, reach out.
### With PO or Designer
- Exchange info with PO or Designer - ask questions to clarify work in tickets, or relative priorities, or whatever.  

## Coding Goals (i.e. Maint / Tech Debt)
- “You're constantly trading off short-term delivering things versus long-term tech debt or investing in platforms of things for the future.” - https://blog.coleadership.com/what-does-it-take-to-be-a-successful-tech-lead/
- “If we only focus on today, we are going to bury ourselves in tech debt or in systems that aren't flexible enough to handle future use cases.” - https://blog.coleadership.com/what-does-it-take-to-be-a-successful-tech-lead/

### Leave the code better than it was when work-cycle began
Balance refactoring, more/better testing and introducing new technologies against desired feature development.
- adherence to best practices
- replace a technology?
- move towards a cleaner/better/more contemporary code structure?  
  - best practices change over time:  OO, rails, rspec
- improve testing?
  - methodology, coverage, ...
- improve maintainability
  - sometimes simple things like a rename or simple refactor make the code more easily understood

## Tickets
(aka github issues)

- Prepare github issues before the work cycle starts, so devs can get started being happy coders.
- Create / edit / comment on issues as new information comes to light
- Ensure open issues intended for devs are actionable by devs
  - are there design or PO decisions that are needed for the work in the ticket to be completed?
    - it is the responsibility of the TL to ensure the necessary information is added to the ticket.
- It's okay to add comments to the ticket like "talk to me before you start coding for this" or "this will require some familiarity with xxx"
- Identify when sign off is needed by the PO or the designer and indicate that in the ticket.

## Board Grooming
This is a big part of being tech lead.  The board is our way of knowing the state of work, our way to determine devs who might benefit from help without realizing they need it (e.g. "Naomi took that ticket but I need to make sure she knows about XXX"), our way to ensure there is always prioritized work available when a developer is looking for a task.

The board can also be a tool for the PO or a Manager:  can they tell what is in progress, what is complete, how issues relate to the Epics (more below) for the work cycle, etc.?

Q: _How often should the TL look at / groom the board?_

A: As often as possible throughout the day. Absolute minimum at 2-3 times / day.  

Q: _What does grooming the board mean?_
- Is the ready column populated with prioritized, actionable work? (more below)
- PRs needing review or re-review (more below)
- Are open PRs associated with issues, if desired?
- Are there issues in the new column that need to be triaged?
- Are tickets in the correct columns?
  - Can any issues be closed / moved to the closed column?
  - Are all in progress tickets properly assigned and in the right column?
  - Are things requiring PO review identified and communicated to PO?
  - Ditto for design review?
- Does the board reflect the current state?  If the

### Ready Column
- Must always be kept populated
  - Depth of at least one ticket per developer
- Each ticket here must be actionable for the developers in the work cycle
  - any open questions for the PO or the designer aren't blocking the work (if so, ticket goes in backlog) ... and are indicated in the ticket
  - needed background information available
  - volume of text in ticket isn't so overwhelming that developer backs away slowly. (Adding structure so it's more skimmable can help)
  - it's okay to add comments to the ticket like "talk to me before you start coding for this" or "this will require some familiarity with xxx"
- Check board for new tickets created by others, ticket order in columns, etc.
- make sure PRs are reviewed promptly, and well.

### Pull Requests

_Quick Review_: It is up to the TL to ensure that PRs are reviewed quickly.  If the TL does a fair number of reviews, this also helps ensure new code is aligned with workcycle coding goals (see below).
- Ideally within 2-3 hours
- More than 24 hours before a review is not good.

_Identify when PO or Designer Should Review Work_: Sometimes the TL has a better feel than the executing developer for when work should get PO sign off.  If you realize this while reviewing the PR, the TL should indicate PO sign off is needed (possibly change the PR to "draft" or at least make it clear in the title that PO sign off is needed (prefix "[hold for PO approval]" or some such)) and be sure to communicate with the PO that their input is desired.

_Before/After Screenshots in PR descriptions Save Time_: In the ETD work-cycle in 2020Q3, the PO was often able to sign off on PRs just from screen shots - faster for the PO to review, and faster turnaround on the PR.  It can also help your fellow developers "see" that your changes did the desired thing:  did change, or didn't change, what is showing in the UI.
- Don't be shy about asking for these if they'll be helpful.

# What Does a TL _AVOID_?
Let's repeat these things here:
- TL is a full-time job; I don't believe you can be a good TL and a developer at the same time because being TL takes at least 70% of work time, and the remaining time is usually broken up into a few minutes here and there.
- The TL role is to help the _other_ devs be as productive as possible on sprint and work-cycle goals.
- Project Management Has to Be Done

TL should NOT:
- expect to write code.  (Obviously, we'd lose a developer if we had long work cycles)
- take any blocker tickets

TL SHOULD:
- only take tiny tickets
- only take tickets not on critical path

_“What am I uniquely capable of doing?”_ - do these things;  leave the rest on the board.
