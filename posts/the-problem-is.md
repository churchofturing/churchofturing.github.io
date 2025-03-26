Picture this. It's 4pm on a Friday and you're stuck in a Teams meeting you're barely paying attention to.
As you're about to phase out of consciousness your coworker drops the coldest line you've ever heard:

> See, the problem is...

You feel butterflies in your stomach at the prospect of the problem finally being identified. You want to cry, laugh, hug a family member, high five a stranger - something to release this overwhelming surge of emotions.

You need a distraction, so you go to [old.reddit.com/r/programming](https://old.reddit.com/r/programming/) and mindlessly click the first post you see. The top comment is from a distinguished scholar you recognise, one u/shartboi420. His comment starts with "I think the problem is...". A cold sweat emerges as you begin to notice a pattern. Two problems identified in one day. Is this even possible?

--- 

What sparked this post was seeing some variation of the phrase "the problem is..." five times in a single work day. When I thought the worst was over me Reddit delivered the coup de grâce. Now I was determined to find out how many problems have been identified on /r/programming and who exactly is the best problem identifier.

First I would need access to the dataset of all comments on /r/programming. At a glance [Pushshift](https://pushshift.io/signup) seemed like the right choice, except they require that you're a moderator and unfortuntely life hasn't been so kind to me yet.

After some digging around I came across this very helpful [post by u/Watchful1.](https://old.reddit.com/r/pushshift/comments/1itme1k/separate_dump_files_for_the_top_40k_subreddits/) It's a torrent with a dump file for the submissions and comments of the top 40 thousand most popular subreddits, exactly what I needed. It seems to be a fairly complete dataset from the start of Reddit until the end of 2024. They even generously included a [script](https://github.com/Watchful1/PushshiftDumps/blob/master/scripts/filter_file.py) that after some slight modification did exactly what I needed.

After downloading the comments for /r/programming and slightly modifying the script to search for occurences of "the problem is", I was finally ready. 

I started the script and n seconds later had produced my life's work.