It's the 18th of August. Today is a special day for me as it marks my one year anniversary of working at $ENTERPRISE.
Before this I had been a professional software developer for the best part of a decade, but entirely in startups and [SMEs](https://en.wikipedia.org/wiki/Small_and_medium_enterprises). This time last year I made the decision to sell out and hit the big leagues for fun and financial profit. 

After my interview the only feedback I recieved was that I hadn't much exposure to enterprise software development, which was true. At the time I took this criticism to heart; only later did I realised it was a compliment. 

In celebration I thought it'd be fun to put some of my observations to paper. 
If you too work at $ENTERPRISE or its relatives and disagree with anything you've read: congratulations and long may it last.

# Things that aren't problems in small businnesses are suddenly intractable problems in large organisations.

On those first days of excitement, I opened my first pull request and was promptly greeted with a big red error generated from a tool I had never heard of for something I didn't do. I asked a developer on my team.

"Hey, have you seen this before?"

"No - this doesn't make much sense. It seems related to the config of $TOOL. Maybe try talking to whoever manages it."

"Can you point me to who that is?"

"Sorry, I've no idea."

At this point I hadn't realised that finding who is responsible for something in a large organisation is very much *not* straightforward. What followed was a week and a half of a Teams-based [call tree](https://en.wiktionary.org/wiki/call_tree) that went precisely nowhere at a snail's pace. By a complete chance I accidentally stumbled over a page in the great forest of Confluence which was from the long ago time when the tool was implemented (two years ago) and owned by a person who had now left (was made redundant). I was told that this means the tool is now rogue; chugging along, costing thousands a month but in effect unsupported. From lack of love it was beginning to fall apart. 

Our solution was to add a line to the $TOOL config in our project that made it ignore everything.

In an alternate universe I lean out from my desk and ask Carol "Hey, what's up with $TOOL?", to which she replies "Oh I've seen this before, what you do is..."

# Penny Foolish *and* Pound Foolish

In my previous life I had saw teams of great people, doing interesting work and solving real problems on budgets that get lost down the back of the sofa at $ENTERPRISE. It was a culture shock trying to reconcile the scale of the monetary waste relative to how long it would take me as an individual to cultivate the equivalent sum. 

Your entire retirement fund spunked in two weeks on a project that was doomed to fail from the offset. Generational wealth being funneled directly to Bezos's mega-yacht via AWS, for workloads that a Raspberry Pi cluster would be overkill for. Hundreds of highly-paid people hours spent debating whether the $100 a month SaaS that underpins half the org is worth keeping. A two year project being cancelled right before launch because it went slightly over budget with senior leadership making the call that we'd save more money going out to tender. Your request for a new mouse has been denied.

# The quality of your coworkers is inconsistent

With small companies, the person (or people) responsible for hiring sets the tone of the entire workplace. If the person responsible for hiring developers is himself a competent developer, he'll *usually* be able to see through the bullshitters. If a bullshitter slips through then they're awkwardly let go as part of their probationary period. For better and worse this maintains a sort of baseline standard.

At $ENTERPRISE, nobody gets fired. Your only risk of job-loss is being "made redundant" as part of the ever changing company structure. The lack of performance related churn creates a self perpetuating competency problem; the inconsistency of new hires reflects the inconsistency of your coworkers. 

This creates incredibly surreal situations like the head of [something technical] not knowing how to use a computer, or an analyst [of something important] not being able to speak English. The reports you recieve aren't really coherent, but there are a lot of em-dashes. You know the absurdity of the situation, they know the absurdity of the situation, but the show must go on until one of you gets made redundant. There's never an incentive to mention these elephants in the room.

What can you do about this? Not a whole lot. 

> Someone had blundered.
>
> \- [Alfred, Lord Tennyson](https://www.poetryfoundation.org/poems/45319/the-charge-of-the-light-brigade)

# Urgency is just a word

In previous jobs I've had, the urgency around work was always very clearly demarkated. 

"You need to finish the website by the end of this month because the client is running a TV advertisement with the URL in it."

Okay, makes sense.

"New regulation comes into effect next quarter, we need to make sure we're compliant before then."

Grand, lets plan it in.

But at $ENTERPRISE, urgency takes many forms. Part of learning the culture of $ENTERPRISE is being able to figure out when the urgency is real, when it isn't and when it's both. 

"I'm going to need you to work this weekend. I committed to an arbitrary date in front of my manager's manager, forgot to tell you, and I'm *guessing* it would look bad if we missed it."

Now, no project manager will ever be this transparent with you. It's up for you to peel back these layers, but some variation of this sentiment has been the overwhelming source of urgency pushed on to me. How you respond is up to you but I'm not working weekends.

"For some reason a firewall rule changed and half of our clients can't connect. I need you to look at this."

This is real urgency, and it's probably in your interests that drop what you're doing and try your best to rectify this. Be transparent about the actions you take, keep people in the loop with your progress and tell yourself nobody will die as a result of this. That is unless you work with safety critical systems, in which case good luck.

"Another part of the organisation failed to tell us that we'll need to make changes to our system to support an upcoming release they have. They're now trying to apply pressure to our team to get their release over the line."

This is Schrödinger's stress. It exists in a super-position of huge amounts of stress and absolutely none at all. The **only difference is if you open the box.** In these situations a good manager is what makes the difference; some will fold to the pressure in the hopes that being a "team player" will get them brownie points from leadership (it wont), and some will stick up for their team out of decency and hoping it will get them brownie points from their colleagues (it will).

# Titles mean very little

I have met no less than 6 (six) people with the title "head of architecture". I still have very little idea what this job role implies.

# Theatre of Security

Software security is one of the most important, subtle and impactful functions in any organisation. There's an art to risk assessment, to balancing pro-activity against reactivity, to the layering of security controls. 

But art doesn't pay the bills, and your manager doesn't particularly care about the subtleties unless you can conjour a number of bad things and show on a graph that the number of bad things are decreasing over time. You look outside your window and see someone in a Wiz.io hoodie with a boombox held above their head. It's the 6th week in a row they've been out there, and you know they have the dashboard filled with special numbers that you can show to your manager. You think it's strange they make you sign the contract in blood, but how bad could it be?

Twice a day you export an excel spreadsheet that has a big list of vulnerable dependencies and forward it to some engineers. The number of bad things starts to go down. You're personally responsible for *thousands* of averted disasters, and everyone claps at the end of your presentations. Every so often you get a little push back from an engineer saying that a vulnerability isn't actually critical given there's no vector for exploitation, or he's not sure how his version of NodeJS is end-of-life given he owns a Python project, but these trivial details matter not. 

After a while the engineers start calling you "dependingly bothersome", which they then shorten to "Dependabot". It must mean you're doing something right.

# Uncertainty is weakness

$ENTERPRISE has been going through massive organisational changes. I've dodged two rounds of redundancies and in that time have seen tens of senior leaders get fired, hired and retired. 

One of the big talking points in $ENTERPRISE is how we're doomed to repeat the same mistakes time after time. The domain is highly complex, filled with pitfalls and failed projects. It stands to reason that for a senior leader, if they wish to avoid the mistakes of the past they would want to put the time in familiarising themselves with the highly complex domain and the mistakes that have gotten us into this pitiful situation.

Here's the rub: to the people senior leaders are accountable to, statements like "I don't know" are effectively taboo. They're hired to hit the ground running, to roll their sleeves up and dig the organisation out of the current pit they're in. The selection process for highly confident senior leaders, mixed with the demand for immediate positive change in a highly complex space creates absolutely *ridiculous* situations. The senior leaders know they're being closely scruitinised from all angles, but the scruitinity from above will always take priority over the scruitiny from below. 

I joined a call with a new senior leader recently after he was hired, and in my naivety was buzzing with the prospect of hearing some fresh ideas. One of the first things they said, editorisalising of course, was:

"You remember that approach we've tried 4 times before that ended in abject failure? We're doing it again, except this time it'll be better because I'm here."

Lesson learned: *If the selection function for senior leadership is tuned to a certain personality type, you're doomed to repeat your mistakes.*

# Engineering teams are empires unto themselves

$ENTERPRISE isn't particularly mature in its engineering standards. With how inconsistent your coworkers are, the application of engineering principles is similarly inconsistent. What surprised me the most were the standards I took for granted just because of the department I was hired into. We have plenty of room for improvement, but all things considered we were doing the Right Things™. 

Then I heard word there are other empires. Some ran by tyrannical rulers with strange idiosyncracies. I began to hear strange whispers, like the next empire over doesn't write any tests and their only quality assurance process was an entire off-shore team manually clicking through the application. Surely that's not true? I had a meeting with their delegates and saw the cultural differences first hand. Their developers are dead behind the eyes. Unquestioning. Apathetic. I returned to my team and relayed my findings like a Software Heroditus. 

It's not a good practice to look down on people, as eventually that critical eye will be turned back on you. Which eventually it did. In interacting with more teams I saw that we weren't the pinnacle of engineering standard, in comparison to other teams further in their journey we were practically crawling out of the bronze age. 

This empirical view is a huge problem, one which I'm guilty of. It's the mechanism that enforces these micro-cultures. 