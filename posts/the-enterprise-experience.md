It's the 18th of August. Today is a special day for me as it marks my one year anniversary of working at $ENTERPRISE.
Before this I had been a professional software developer for the best part of a decade, but entirely in startups and [SMEs](https://en.wikipedia.org/wiki/Small_and_medium_enterprises). I made the decision to sell out and hit the big leagues for fun and financial profit. 

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

With small companies, the person (or people) responsible for hiring set the tone of the entire workplace. If the person responsible for hiring developers is himself a competent developer, he'll *usually* be able to see through the bullshitters. If a bullshitter slips through then they're awkwardly let go as part of their probationary period. For better and worse this maintains a sort of baseline standard.

In $ENTERPRISE, nobody gets fired. Your only risk of job-loss is being "made redundant" as part of the ever changing company structure. The lack of performance related churn creates a self perpetuating competency problem; the inconsistency of new hires reflects the inconsistency of your coworkers. 

This creates incredibly surreal situations like the head of [something technical] not knowing how to use a computer, or an analyst [of something important] not being able to speak English. You know the absurdity of the situation, they know the absurdity of the situation, but the show must go on until one of you gets made redundant. There's never an incentive to mention these elephants in the room.

What can you do about this? Not a whole lot. 

> Someone had blundered.
>
> Theirs not to make reply,
>   
> Theirs not to reason why,
>
> Theirs but to do and die.
>
> \- [Alfred, Lord Tennyson](https://www.poetryfoundation.org/poems/45319/the-charge-of-the-light-brigade)

# Urgency is just a word

# Theatre of Security