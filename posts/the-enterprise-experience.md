# The Enterprise Experience

It's the 18th of August. Today is a special day for me, as it marks my one-year anniversary of working at $ENTERPRISE.
Before this I had been a professional software developer for the best part of a decade, but entirely in startups and [SMEs](https://en.wikipedia.org/wiki/Small_and_medium_enterprises). This time last year I made the decision to sell out and hit the big leagues for fun and financial profit. 

After my interview the only feedback I received was that I didn't have much exposure to enterprise software development, which was completely true. At the time I took this criticism to heart; only later did I realise it was a compliment. 

In celebration I thought it'd be fun to put some of my observations to paper. They're not so much observations as just unfiltered snark, but I'm enjoying the cathartic feeling of getting it out.

If you too work at $ENTERPRISE or its relatives and disagree with anything you've read, congratulations and long may it last.

## Things that aren't problems in small businesses are suddenly intractable problems in large organisations

On those first days of excitement, I opened my first pull request and was promptly greeted with a big red error generated from a tool I had never seen before for something I didn't do. I asked a developer on my team, and the conversation went something like this:

*"Hey, have you seen this before?"*

"No - this doesn't make much sense. It seems related to the config of $TOOL. Maybe try talking to whoever manages it."

*"Can you point me to who that is?"*

"Sorry, I've no idea."

At this point I hadn't realised that finding who is responsible for something in a large organisation is very much *not* straightforward. What followed was a week and a half of a Teams-based [call tree](https://en.wiktionary.org/wiki/call_tree) that went precisely nowhere at a snail's pace. By a complete chance I accidentally stumbled over a page in the great forest of Confluence which was from the long ago time when the tool was implemented (two years ago) and owned by a person who had now left (was made redundant). I was told that this means the tool is now rogue; chugging along, costing thousands a month but in effect unsupported. From lack of love it was beginning to fall apart. 

Our solution was to override the global $TOOL config by adding a line to the config in our project that made it ignore everything.

In an alternate universe I lean out from my desk and ask Carol, "Hey, what's up with $TOOL?", to which she replies, "Oh I've seen this before, what you do is..."

## Penny foolish *and* pound foolish

In my previous life I had seen teams of great people doing interesting work and solving real problems on budgets that get lost down the back of the sofa at $ENTERPRISE. It is and was a culture shock trying to reconcile the scale of the monetary waste relative to how long it would take me as an individual to cultivate the equivalent sum. 

Your entire retirement fund spunked in two weeks on a project that was doomed to fail from the offset. Generational wealth being funnelled directly to Bezos's mega-yacht via AWS, for workloads that a Raspberry Pi cluster would be overkill for. Hundreds of highly paid people spending hours debating whether the $100 a month SaaS that underpins half the org is worth keeping. A two-year project being cancelled right before launch because it went slightly over budget, with senior leadership making the call that we'd save more money going out to tender. Your request for a new mouse has been denied.

## Inconsistent coworkers

With small companies, the person (or people) responsible for hiring sets the tone of the entire workplace. If the person responsible for hiring developers is himself a competent developer, he'll *usually* be able to see through the bullshitters. If a bullshitter slips through then they're awkwardly let go as part of their probationary period. For better or worse this maintains a sort of baseline standard.

At $ENTERPRISE, nobody gets fired. Your only risk of job loss is being "made redundant" as part of the ever-changing company structure. The lack of performance related churn creates a self perpetuating competency problem; the inconsistency of new hires reflects the inconsistency of your coworkers. 

This creates incredibly surreal situations like the head of [something technical] not knowing how to use a computer, or an analyst [of something important] not being able to speak English. The reports you receive aren't really coherent, but there are a lot of em dashes. You know the absurdity of the situation, they know the absurdity of the situation, but the show must go on until one of you gets made redundant. There's never an incentive to mention these elephants in the room.

What can you do about this? Not a whole lot. 

> Someone had blundered.
>
> \- [Alfred, Lord Tennyson](https://www.poetryfoundation.org/poems/45319/the-charge-of-the-light-brigade)

## 'Urgency' is just a word

In the previous jobs I've had, the urgency around work was always very clearly demarcated. 

"You need to finish the website by the end of this month because the client is running a TV advertisement with the URL in it."

Okay, makes sense.

"New regulation comes into effect next quarter; we need to make sure we're compliant before then."

Grand - let's plan it in.

But at $ENTERPRISE, urgency takes many forms. Part of learning the culture is being able to figure out when the urgency is real, when it isn't and when it's a mix of both. 

"I'm going to need you to work this weekend. I committed to an arbitrary date in front of my manager's manager, forgot to tell you, and I'm *guessing* it would look bad if we missed it."

Admittedly, no project manager will ever be this transparent with you. It's up to you to peel back these layers, but some variation of this sentiment has been the overwhelming source of urgency pushed onto me. How you respond is up to you, but I've never seen the guy working weekends get rewarded for it.

"For some reason a firewall rule changed and half of our clients can't connect. I need you to look at this."

This is real urgency, and it's probably in your interests to drop what you're doing and try your best to rectify this. Be transparent about the actions you take, keep people in the loop with your progress and tell yourself nobody will die as a result of this. That is unless you work with safety-critical systems, in which case good luck.

"Another part of the organisation failed to tell us that we'll need to make changes to our system to support an upcoming release they have. They're now trying to apply pressure to our team to get their release over the line."

This is the interesting case of Schrödinger's urgency. It exists in a superposition of huge amounts of stress and absolutely none at all. I cannot emphasise enough that the **only difference is if you open the box.** In these situations a good manager is what makes the difference; some will fold to the pressure in the hopes that being a "team player" will get them brownie points from leadership (it wont), and some will stick up for their team out of decency and hoping it will get them brownie points from their colleagues (it will).

## Theatre of Security

Software security is one of the most important, subtle and impactful functions in any organisation. There's an art to risk assessment, to balancing proactivity against reactivity, to the layering of security controls. 

But art doesn't pay the bills, and your manager doesn't particularly care about the subtleties unless you can conjure a number of bad things and show on a graph that the number of bad things is decreasing over time. You look outside your window and see someone in a Wiz.io hoodie with a boombox held above their head. It's the 6th week in a row they've been out there, and you know they have the dashboard filled with special numbers that you can show to your manager. You think it's strange they make you sign the contract in blood, but how bad could it be?

Twice a day you export an Excel spreadsheet that has a big list of vulnerable dependencies and forward it to some engineers. The number of bad things starts to go down. You're personally responsible for *thousands* of averted disasters, and everyone claps at the end of your presentations. Every so often you get a little pushback from an engineer saying that a vulnerability isn't actually critical given there's no vector for exploitation, or he's not sure how his version of NodeJS is end-of-life given he owns a Python project, but these trivial details matter not. 

After a while the engineers start calling you "dependingly bothersome", which they then shorten to "Dependabot". It must mean you're doing something right.

This is just one contrived example of the general pitfall of turning everything into a metric and working in service of the metrics. In certain leadership structures this can take you incredibly far, but remember that the *menu* is not the *meal*. 

## Titles mean very little

I have met no less than 6 (six) people with the title "head of architecture". I still have no idea what this job role implies.

## Uncertainty is weakness

$ENTERPRISE has been going through massive organisational changes. I've dodged two rounds of redundancies and in that time have seen tens of senior leaders get fired, hired and retired. 

One of the big talking points in the company is how we're doomed to repeat the same mistakes time after time. The domain is highly complex, filled with quicksand and failed projects. It stands to reason that for a senior leader, if they wish to avoid the mistakes of the past they would want to invest the time in familiarising themselves with the highly complex domain and the mistakes that have gotten us into this pitiful situation.

Here's the rub: to the people senior leaders are accountable to, statements like "I don't know" are effectively taboo. They're hired to hit the ground running, to roll their sleeves up and dig the organisation out of the current mess we're in. The selection process for highly confident senior leaders, mixed with the demand for immediate positive change in a highly complex space is contradictory. The senior leaders know they're being closely scrutinised from all angles, but the scrutiny from above will always take priority over the scrutiny from below. 

I joined a call with a new senior leader recently after they were hired, and, in my naivety, was buzzing with the prospect of hearing some fresh ideas. One of the first things they said (editorialising of course) was:

"You remember that approach we've tried 4 times before that ended in abject failure? We're doing it again, except this time it'll be better because I'm here."

Lesson learnt: *If the selection function for senior leadership is tuned to a certain personality type, you're doomed to repeat your mistakes.*

# Engineering teams are empires unto themselves

$ENTERPRISE isn't particularly mature in its engineering standards. With how inconsistent your coworkers are, the application of engineering principles is similarly inconsistent. What surprised me the most were the standards I took for granted just because of the department I was hired into. We have plenty of room for improvement, but all things considered we were doing the Right Things™. 

Then I heard word there are other empires. Some were run by tyrannical rulers with strange idiosyncrasies. I began to hear strange whispers, like the next empire over doesn't write any tests, and their only quality assurance process was an entire off-shore team manually clicking through the application. Or that an empire in a distant land has pyramids of software that touch the sky, crafted by thousands of people over decades.

Much like real empires, this way of viewing an organisation comes with a huge amount of issues and is something that should be actively resisted. Breaking down these barriers is easier said than done; the dominant empire rarely wants to lose their position, and empires in general are rarely in favour of lessening their autonomy. 

## The positives

I find it very easy to be sardonic, but I don't want this post to come across as a *purely* bitter rant. I've actually very much enjoyed the last year and have zero regrets with my move. It's just the positives aren't as fun to rant about.

Here's an unordered list of the things I've appreciated over the last year:

- Being plugged in to an engineering community has helped refine my views on software development.
- There are actual opportunities for career development.
- It's satisfying to write software used by millions of people.
- The mentorship opportunities in learning from those more experienced and being able to guide those more junior.
- As paradoxically as it sounds, aside from the rounds of redundancies the job security feels quite good. 
- Getting paid on time.
- Being given the space to specialise instead of being expected to be a generalist in all areas.
- The diversity of the team. Small organisations can be quite homogeneous in views/attitudes.
- Upskilling is encouraged and training is protected. 

Maybe I'll come back in 9 years and see how my views have changed.

Feel free to get in touch with me at: churchofturing@gmail.com.