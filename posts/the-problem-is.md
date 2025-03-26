# The Problem Is...

Picture this. It's 4pm on a Friday and you're stuck in a Teams meeting you're barely paying attention to.
As you're about to phase out of consciousness your coworker drops the coldest line you've ever heard:

> "See, the problem is..."

You feel butterflies in your stomach at the prospect of the problem finally being identified. You want to cry, laugh, hug a family member, high five a stranger - something to release this overwhelming surge of emotions.

You need a distraction, so you go to [old.reddit.com/r/programming](https://old.reddit.com/r/programming/) and mindlessly click the first post you see. The top comment is from a distinguished scholar you recognise, one u/shartboi420. His comment starts with "I think the problem is...". A cold sweat emerges as you begin to notice a pattern. Two problems identified in one day. Is this even possible?

--- 

What sparked this post was seeing some variation of the phrase "the problem is..." five times in a single work day. When I thought the worst was over me Reddit delivered the coup de grâce. Now I was determined to find out how many problems have been identified on /r/programming and who exactly is the best problem identifier.

First I would need access to the dataset of all comments on /r/programming. At a glance [Pushshift](https://pushshift.io/signup) seemed like the right choice, except they require that you're a moderator and unfortuntely life hasn't been so kind to me yet.

After some digging around I came across this very helpful [post by u/Watchful1.](https://old.reddit.com/r/pushshift/comments/1itme1k/separate_dump_files_for_the_top_40k_subreddits/) It's a torrent with a dump file for the submissions and comments of the top 40 thousand most popular subreddits, exactly what I needed. It seems to be a fairly complete dataset from the start of Reddit until the end of 2024. They even generously included a [script](https://github.com/Watchful1/PushshiftDumps/blob/master/scripts/filter_file.py) that after some slight modification did exactly what I needed.

After downloading the comments for /r/programming and slightly modifying the script to search for occurences of "the problem is", I was finally ready. 

I started the script and 81 seconds later had produced my life's work. [the_problem.csv](the_problem.csv). It's a CSV containing every problem identified on /r/programming, around **36,267** in total. The overall number of comments scanned was **8,056,545**, meaning that comments containing "the problem is" make up approximately **0.45%** of all /r/programming comments. I'm not sure if this is higher or lower than I expected, but it still feels like too many.

So now I know how many problems have been identified, but now I want to know who's identifying them. So lets get the top 10 problem identifiers (including u/[deleted]). Fortunately this was trivial using Python.

```
import csv
from collections import Counter

counts = Counter()

with open("the_problem.csv", "r", encoding="utf-8") as f:
    reader = csv.reader(f)
    for row in reader:
        counts[row[2]] += 1

# The top is always u/[deleted].
# So we get the top 11 instead.
top_11 = counts.most_common(11)

for value, count in top_11:
    print(f"{value}: {count}")
```

Drumroll please...

1. u/grauenwolf: 298

2. u/lookmeat: 204

3. u/dnew: 184

4. u/G_Morgan: 175

5. u/yogthos: 175

6. u/matthieum: 136

7. u/pron98: 130

8. u/killerstorm: 107

9. u/mirhagk: 107

10. u/MarshallBanana: 103

Congratulations to [u/grauenwolf](https://old.reddit.com/user/grauenwolf), you're the winner of the coveted title of Chief Problem Identifier! The problem is I have nothing to give you. Also well done to our runner ups, keep at it and we'll be out of problems in no time.

---

* Yes, this is a non-serious shitpost born out of frustration at a common turn of phrase.
* You're right, it isn't rigorous and I didn't accomodate for false positives and quoted replies.