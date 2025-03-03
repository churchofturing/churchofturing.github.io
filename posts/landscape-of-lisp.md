Lisp is a family of programming languages with a long, rich and oftentimes confusing tradition.
Unware of it at the time, in 1960 John McCarthy published [a paper](https://www-formal.stanford.edu/jmc/recursive.pdf) 
that over the next 64 years would spawn a thousand dialects and a number of *very* good ideas.

The fragmentation of the Lisp family from an outside perspective can seem really confusing to newcomers, like a software Tower of Babel. In fact the variety of Lisp is in my opinion one of it's best qualities.
It almost guarantees that as long as you can overcome the (relatively) small barriers 
of the paren-heavy syntax and prefix notation that there will be a dialect and community that 
closely mirrors your philosophy on software development. 

The purpose of this post isn't to convince the reader of how great Lisp is; 
rather, I assume the reader is already somewhat interested and is now trying to 
figure out which of these dialects is the best fit for them. It also isn't about "which Lisp is the best" type questions, but instead it's just my subjective 
view of the more prominent Lisp dialects. 

Tl;dr:
- If you want a minimalist, elegant Lisp with a strong academic foundation: choose Scheme.
- If you want a powerful, batteries-included Lisp with a rich standard library and decades of history: choose Common Lisp.
- If you want a modern Lisp with functional programming, concurrency, and JVM interop: choose Clojure.
- If you want a beginner-friendly Lisp with great tooling and a focus on education and extensibility: choose Racket.

If the reader isn't already convinced but is still curious, I would recommend reading the following articles:

- [Beating the Averages - Paul Graham](https://paulgraham.com/avg.html)
- [Lisp: Good News, Bad News, How to Win Big - Richard P. Gabriel](https://dreamsongs.com/WIB.html)
- [The Art of Lisp & Writing - Richard P. Gabriel](https://dreamsongs.com/ArtOfLisp.html)
- [Programming Bottom-Up - Paul Graham](https://paulgraham.com/progbot.html) 
- [How To Become A Hacker - Eric Steven Raymond](http://www.catb.org/%7Eesr/faqs/hacker-howto.html)
- [How Lisp Became God's Own Programming Language - Sinclair Target](https://twobithistory.org/2018/10/14/lisp.html)


# Culture

I mentioned earlier that Lisp's diversity is one of it's best qualities, and I stand by that. A side effect of this 
however is that inter-dialect tribalism is unfortunately common. It's a weird reality that the smallest of differences 
seem to incite the most vitriol, whether it's with programming language dialects or [Northern Conservative Baptism.](https://www.youtube.com/watch?v=l3fAcxcxoZ8)

I've noticed this kind of anger express itself more when people start to blur the lines between their interests
and their identity. At this point it's difficult to *not* take technical disagreement as a personal insult. If you do 
start considering yourself a Lisper, remember that we're all more similar than we are different
and there's plenty of room for a variety of opinion. 

Not to leave on a sour note, Lisp also attracts some of the most thoughtful and intelligent people whose willingness
to openly share their knowledge has changed me for the better. 

# History

A quick aside. It can be hard to understand the design decisions made by Lisps without knowing at least a bit about their
evolutionary history. Why do humans get goosebumps? Why is Emacs Lisp dynamically scoped? The best way of getting 
a sense of how Lisp has evolved is to reference the numerous papers written at various times.

This isn't at all necessary to picking up a Lisp by the way, I just think it's neat.

- [Lisp 1.5 Programmer's Manual  John McCarthy et al. (1962)](https://www.softwarepreservation.org/projects/LISP/book/LISP%201.5%20Programmers%20Manual.pdf)
- [History of Lisp - John McCarthy (1979)](http://jmc.stanford.edu/articles/lisp/lisp.pdf)
- [The Evolution of Lisp - Richard P. Gabriel, Guy L. Steele Jr (1993)](https://www.dreamsongs.com/Files/HOPL2-Uncut.pdf)
- [The History of Lisp - Herbert Stoyan](https://www.softwarepreservation.org/projects/LISP/book/Stoyan-Geschichte.pdf)
- [History Of Lisp - Paul McJones](https://www.softwarepreservation.org/projects/LISP/)
- [Lisp History Resources - Paul McJones](https://www.softwarepreservation.org/projects/LISP/resource/)

# Scheme

[Scheme](https://www.scheme.org/) is a minimalist Lisp [created at MIT](https://standards.scheme.org/official/r0rs.pdf) in the 70's 
by [Guy L. Steele Jr.](https://en.wikipedia.org/wiki/Guy_L._Steele_Jr.) and 
[Gerald Jay Sussman](https://en.wikipedia.org/wiki/Gerald_Jay_Sussman). It began as a series of 
[papers](https://research.scheme.org/lambda-papers/), then eventually a [report](https://dspace.mit.edu/bitstream/handle/1721.1/5794/AIM-349.pdf), and
subsequently evolved as revisions to the report were made.
 
The minimalism of Scheme makes it well suited both as a teaching language and an embedded extension language in other programs. Scheme's minimalism 
also makes it attractive to Lisp implementers, and as a result there is a [rich ecosystem](http://community.schemewiki.org/?category-implementations) of Scheme 
[compilers and interpreters](https://get.scheme.org/).

I've quite the soft spot for Scheme - it was the first Lisp I learned via SICP, and the programs I've written and 
read tend to have an elegance that's hard to find elsewhere. There are few programming languages I would describe as 
beautiful but Scheme is definitely in that list (pun intended).

Scheme's notable for a few different reasons:
- A minimalist design with a small but powerful standard library.
- It was the first Lisp to choose lexical scoping as a default. 
- It supports first class continuations.
- It facilitates functional programming by requiring implementations to support tail-call optimisation.
- There are a *lot* of Scheme implementations, each with their own tradeoffs and benefits. 
- It was the teaching language used by Ableson and Sussman in their classic "Structure and Interpretation of Computer Programs".

Despite the simplicity of Scheme itself, everything else around it can seem complicated and daunting
 at a first glance. The next few paragraphs are what I would consider essential context.


The 'Revisedⁿ Report on the Algorithmic Language Scheme', 
(typically abbreviated RⁿRS) is the de-facto Scheme language specification. As of now there is [1 original report](https://codeberg.org/scheme/r7rs/wiki#report-on-scheme-1975) 
and 7 revision documents. I suppose technically 8 revisions given the most recent revision has been split in two. 
The most commonly talked about [standards](https://standards.scheme.org/) are also the most recent: [R5RS (1998)](https://standards.scheme.org/official/r5rs.pdf), 
[R6RS (2007)](https://standards.scheme.org/official/r6rs.pdf), [R7RS-small (2013)](https://standards.scheme.org/official/r7rs.pdf) and 
[R7RS-large (ongoing)](https://codeberg.org/scheme/r7rs). As mentioned previously R7RS is divided into two parts: ["...a small language primarily for embedding, education, and research; 
and a large language to address the practical needs of mainstream software development."](https://r7rs.org/)

Now this is where things start to get a little hairy. R6RS was (or is) very controversial in the Scheme community
for [various reasons](https://small.r7rs.org/wiki/SixRejection/3/) that I wont get into here, but to quote Gwen Weinholt's summary from their 
[post on the topic](https://weinholt.se/articles/r7rs-vs-r6rs/):

> R6RS is more demanding on implementers but easier on users. Conversely, R7RS is easier on implementers but more demanding on users.

This unsurprisingly resulted in more [implementations](https://get.scheme.org/) supporting R7RS than R6RS.  

Another important thing to understand about Scheme are ["SRFIs" (Scheme Requests for Implementations)](https://srfi.schemers.org/).
Due to Scheme's inherent minimalism and many implementations, coordinating library proposals became essential for
 writing portable code. There are a bunch of SRFIs and each Schemer will typically have a set of them 
they commonly use, and I would recommend everyone to have a look through them.

So between RnRS's, SRFIs and a smorgasbord of implementations we find ourselves in a weird position.
- The language has *multiple* standards.
- The language has *many* implementations.
- The implementations can support *one or many* standards.
- The implementations can (and often do) *extend the language* in non-standard ways.
- The implementations can *choose what SRFIs* to support.

Due to this complexity and [the challenges the R7RS-large](https://dpk.land/io/r7rswtf) working group have faced when 
reaching agreement, it's not unusual to see expressed the feeling that Scheme as a standard 
is dying. I'm not sure I fully believe this myself but only time will tell.

If you're new to Scheme don't let this discourage you for a second. The trick is to not think about it too much; it's perfectly fine to pick an implementation 
and just start having fun hacking.

One last thing - Scheme's minimalism makes it great for embedding as an extension language in other programs. See [Chibi-Scheme](https://github.com/ashinn/chibi-scheme) for an example of this.

## Writings

The writings and videos below are either explicitly about Scheme or incidentally use Scheme as a teaching tool.

Classics:

- [The Lambda Papers - Guy L. Steele Jr. and Gerald Jay Sussman](https://research.scheme.org/lambda-papers/)
- [Structure and Interpretation of Computer Programs (SICP) - Hal Abelson and Gerald Jay Sussman](https://mitp-content-server.mit.edu/books/content/sectbyfn/books_pres_0/6515/sicp.zip/index.html)
  - SICP isn't just a Scheme classic but an all time classic.
- [The Little Schemer - Daniel P. Friedman and Matthias Felleisen](https://felleisen.org/matthias/BTLS-index.html)
- [The Seasoned Schemer - Daniel P. Friedman and Matthias Felleisen](https://felleisen.org/matthias/BTSS-index.html)
- [Software Design for Flexibility - Chris Hanson and Gerald Jay Sussman](https://mitpress.mit.edu/9780262045490/software-design-for-flexibility/)

There are a *lot* of high quality Scheme learning resources online, many of them free too.

- [Simply Scheme - Brian Harvey and Matthew Wright](https://people.eecs.berkeley.edu/~bh/ss-toc2.html)
- [Scheme and the Art of Programming - George Springer and Daniel P. Friedman](https://www.amazon.co.uk/Scheme-Art-Programming-G-Springer/dp/0262192888)
- [Teach Yourself Scheme in Fixnum Days - Dorai Sitaram](http://ds26gte.github.io/tyscheme/index.html)
- [The Adventures of a Pythonista in Schemeland - Michele Simionato](https://www.artima.com/weblogs/viewpost.jsp?thread=251474)

Misc:

- [The evolution of a Scheme programmer](https://erkin.party/blog/200715/evolution/)
- [Learn X in Y where X=Chicken](https://learnxinyminutes.com/docs/chicken/)
- [Community Scheme Wiki](http://community.schemewiki.org/)


## Videos

- [Growing a Language - Guy L. Steele Jr.](https://www.youtube.com/watch?v=_ahvzDzKdB0)
  - Not necessarily about Scheme, but relevant in ways I hope are obvious.
- [MIT 6.001 SICP (1986)](https://www.youtube.com/playlist?list=PLE18841CABEA24090)
  - The classic 1986 SICP lecture series.
- [Scheme: Feel the Cool - Andy Balaam](https://www.youtube.com/playlist?list=PLgyU3jNA6VjRMB-LXXR9ZWcU3-GCzJPm0)
- [R7RS Large Status and Progress - Daphne Preston-Kendal](https://www.youtube.com/watch?v=BMvurzOj9ww)
  - Daphne Preston-Kendal is the [chair of the R7RS working group 2](https://codeberg.org/scheme/r7rs/wiki). She's 
  really good at communicating the state of R7RS, and being chair is a hard task but I'm glad she's doing it.
- [The Most Beautiful Program Ever Written - William Byrd](https://www.youtube.com/watch?v=OyfBQmvr2Hc)
- [Unlock Lisp / Scheme's magic: beginner to Scheme-in-Scheme in one hour - Christine Lemmer-Webber](https://www.youtube.com/watch?v=DDROSL-gGOo)
  - A very fun introduction to Scheme for beginners. 
- [Programming Should Eat Itself - Nada Amin ](https://www.youtube.com/watch?v=SrKj4hYic5A)
- [Knit, Chisel, Hack: Building Programs in Guile Scheme - Andy Wingo](https://www.youtube.com/watch?v=uwiaT3MoDVs)

## Community

Scheme has a relatively small but [dedicated community](https://community.scheme.org/). If you're coming from a more popular 
language like JavaScript it might surprise you that discussions usually happen over days 
instead of minutes. This is perfectly normal.

- [/r/scheme on Reddit](https://old.reddit.com/r/scheme/)
- [comp.lang.scheme](https://comp.lang.scheme.narkive.com/)
  - Pretty quiet these days and sometimes spammed, but contains a lot of interesting
  historic discussions.
- IRC: #scheme (and #lisp) on [Libera.Chat](https://libera.chat/)
  - There are various implementation specific channels on Libera too.
- [Scheme Discord](https://discord.gg/ZcTYrdx)


# Common Lisp

If Scheme is often seen as minimal and academic, Common Lisp in contrast could be considered robust and industrial. 
Their histories play a significant role in this perception and the history of Common Lisp is quite an interesting one - the curious reader can find out more about it [here](https://www.dreamsongs.com/Files/Hopl2.pdf).
Summarising the best I can, in the early 80's Lisp was highly fragmented. ARPA weren't that interested in funding multiple projects with 
incompatible Lisp implementations, so they brought the kingdoms together and from this meeting grew the idea of 
a common Lisp to rule them all. To this day if someone directly refers to "Lisp" but not in the broad language-family sense, 
they're probably talking about Common Lisp. 

Common Lisp (CL) was ANSI standardised in 1994 and since then has remained remarkably stable. 
If I needed to write a piece of software I knew wouldn't give me hassle in 30 years I'd choose Common Lisp. 
This level of stability is almost unthinkable to a great deal of software developers. 

Something to note: just because CL is stable doesn't mean it's stagnant. 
There has been a great deal of effort spent on improving the many [Common Lisp implementations](https://lisp-lang.org/wiki/article/implementations), 
improving the library ecosystem, improving the tooling etc. If you're coming from a larger and faster moving ecosystem don't 
be surprised when you see libraries that were last updated 10 years ago. They're not dead, they're just sleeping - if you wake 
them up they'll work just as well as the day they were written.

I mentioned previously that Common Lisp has many implementations, but the most prominent of them is probably 
[Steel Bank Common Lisp (SBCL)](http://www.sbcl.org/). SBCL is great, and here's why:
- It's [very fast](http://web.archive.org/web/20211018040529mp_/https://programming-language-benchmarks.vercel.app/lisp). All the [benchmarks](https://benchmarksgame-team.pages.debian.net/benchmarksgame/measurements/sbcl.html) I've seen blow similar dynamically typed and garbage collected languages out of the water.
- It has a powerful and [highly interactive debugger](https://lispcookbook.github.io/cl-cookbook/debugging.html).
- It comes with [sophisticated profiling tools](https://lispcookbook.github.io/cl-cookbook/performance.html#know-your-lisps-statistical-profiler).
- It provides support for [threading.](https://lispcookbook.github.io/cl-cookbook/process.html)
- It's [image based](https://old.reddit.com/r/lisp/comments/go3dzr/what_is_image_based_programming/frfg8dp/) (google it).

I'm not here to evangalise SBCL but it really is an awesome tool.

Something worth mentioning is programming style. Lisps in general are often described as being "functional" languages and this (especially historically) is a huge misconception. Common Lisp is incredibly unopinionated and leaves it entirely up to the programmer to choose how they want to use it. Want to write heavily procedural code? Great! More of a functional programmer? That's awesome too. Huge OOP fan? The Common Lisp Object System (CLOS) is one of the most sophisticated and robust out there. 

## Writings

Despite not considering myself a Common Lisper, it's a fact that some of the most eye-opening books written about Lisp use Common Lisp as their mode of teaching. 

- [CLHS - Common Lisp Hyperspec](https://www.lispworks.com/documentation/HyperSpec/Front/Contents.htm)
  - The language reference. You'll get familiar with this quick enough.
- [CL Community Spec](https://cl-community-spec.github.io/pages/index.html)
  - Like the hyperspec but nicer.
- [Common Lisp Cookbook](https://lispcookbook.github.io/cl-cookbook/)
- [Practical Common Lisp - Peter Seibel](https://gigamonkeys.com/book/)
  - The quickest way to get up to speed with Common Lisp if you have previous programming experience.
- [ANSI Common Lisp - Paul Graham](https://paulgraham.com/acl.html)
- [Common Lisp Recipes - Edmund Weitz](https://weitz.de/cl-recipes/)
- [On Lisp - Paul Graham](https://paulgraham.com/onlisp.html)
  - *The* book on advanced lisp techniques.
- [Let Over Lambda - Doug Hoyte](https://letoverlambda.com/)
  - Spiritual success to "On Lisp", contains deep and interesting macrology. For the advanced Lisper.
- [The Art of the Metaobject Protocol - Gregor Kiczales](https://en.wikipedia.org/wiki/The_Art_of_the_Metaobject_Protocol)
  - A deep dive into CLOS. Also not for the faint of heart.
- [Common Lisp: A Gentle Introduction to Symbolic Computation - David S. Touretzky](https://www.cs.cmu.edu/~dst/LispBook/)
- [Land of Lisp – Conrad Barski](http://landoflisp.com/)
- [CLiki: The Common Lisp Wiki](https://www.cliki.net/)

## Videos

- [Little Bits of Lisp - Baggers](https://www.youtube.com/watch?v=m0TsdytmGhc&list=PL2VAYZE_4wRJi_vgpjsH75kMhN4KsuzR_)
- [Common Lisp programming: from novice to effective developer - Vincent Dardel](https://www.udemy.com/course/common-lisp-programming/)
- [Google Talk: Practical Common Lisp - Peter Seibel](https://www.youtube.com/watch?v=VeAdryYZ7ak)
- [Growing a Language - Guy L. Steele Jr](https://www.youtube.com/watch?v=_ahvzDzKdB0)
  - Not specifically about Common Lisp, but it's too good to not mention.
- [Gavin Freeborn](https://www.youtube.com/@GavinFreeborn/videos)
  - Great YouTube channel.
- [Common Lisp Series - Neil Munro](https://www.youtube.com/watch?v=xyXDE5gP2QI&list=PLCpux10P7KDKPb4eI5b_qSnQaY1ePGKGK)
- [Land of Lisp - The Music Video](https://www.youtube.com/watch?v=HM1Zb3xmvMc)
- [BALISP playlist](https://www.youtube.com/playlist?list=PLoH3jteqsb2h-F5AHG4XVyZ6GwODX4uVB)

## Community

Common Lisp has a pretty small yet dedicated community. From what I've seen from an outside perspective, there's quite a few regional Common Lisp groups dotted around the world. A first step would be to see if there's any in your area.

- [/r/Common_Lisp](https://old.reddit.com/r/Common_Lisp/)
- [CL Discord](https://discord.com/invite/hhk46CE)
- [European Lisp Symposium](https://european-lisp-symposium.org/index.html)
- [Toronto Lisp Users Group](https://torlisp.neocities.org/)
- [Atlanta Functional Programming](https://www.youtube.com/@atlantafunctionalprogrammi6401/featured)
- [comp.lang.lisp](https://groups.google.com/g/comp.lang.lisp)
  - Mostly spammed to death these days, very interesting as an historical record. Reading through the archived posts and seeing 30 year old flame wars is always fun.
  - If you're interested in the reputation comp.lang.lisp used to have, read this [19 year old blog post](https://eli.thegreenplace.net/2006/10/27/the-sad-state-of-the-lisp-user-community).
- [IRC: #lisp, #commonlisp on Libera.Chat](https://libera.chat/)
- [Awesome Common Lisp](https://github.com/CodyReichert/awesome-cl)

# Clojure

Clojure is the most recent of the language discussed so far, and since its appearance in 2007 has spawned a sort of mini-renaissance of renewed Lisp interest. It's also the Lisp I'm most fond of for my own personal projects.

Here's some points that set Clojure apart:

- Runs on the JVM and can therefore interop with the Java ecosystem.
- Encourages a functional programming style with immutable data structures.
- Provides a variety of basic data structures: map, vectors, sets, lists.
  - Most Lisps only provide one or two. Opinions are divided on whether this is a good thing or not.
- The focus on immutable data structures and [software transactional memory](https://clojure.org/reference/refs) makes it very pleasant to use for writing concurrent software.
- A comparatively healthy library ecosystem for writing web services.

Something else that sets Clojure apart is that its creator, Rich Hickey, is still heavily involved in the language and its community. This is in contrast to other Lisps which tend to be much more committee and community driven. In the Clojure community you'll see frequent reference to Rich's talks of which there's a good few. They're all worth watching in my opinion.

Oh, and there's also [ClojureScript](https://clojurescript.org/). It's Clojure that compiles to JavaScript - I haven't used it much but if that interests you, go for it.

## Writings

Clojure has quite a lot written about it. I'd typically just recommend looking around until you find a resource that matches your tastes.

- [Practical Clojure - Luke VanderHart](https://www.amazon.co.uk/Practical-Clojure-Experts-Voice-Source/dp/1430272317)
  - It's quite an early book having been published in 2010. It'll still be largely relevant but will miss some advanced features such as transducers, spec etc. 
- [Clojure from the Ground Up - Kyle Kingsbury](https://github.com/ligurio/clojure-from-the-ground-up)
- [Elements of Clojure - Zachary Tellman](https://elementsofclojure.com/)
- [Clojure for the Brave and True - Daniel Higginbotham](https://www.braveclojure.com/)
- [Clojure Koans - Aaron Bedra et al](https://github.com/functional-koans/clojure-koans)
- [Learn X in Y where X = Clojure](https://learnxinyminutes.com/clojure/)
- [An Opinionated List of Excellent Clojure Learning Materials - Srihari Sriraman](https://gist.github.com/ssrihari/0bf159afb781eef7cc552a1a0b17786f)
- [Awesome Clojure - Michał Buczko](https://github.com/mbuczko/awesome-clojure)
- [A History of Clojure - Rich Hickey](https://clojure.org/about/history)
- [ClojureDocs](https://clojuredocs.org/)

## Videos

In my opinion, there's not a whole lot of really interesting Clojure talks other than the handful from Rich. 

- [Simple Made Easy - Rich Hickey](https://youtu.be/LKtk3HCgTa8)
- [Clojure for Java Programmers Part 1 - Rich Hickey](https://www.youtube.com/watch?v=P76Vbsk_3J0)
- [Clojure for Java Programmers Part 2 - Rich Hickey](https://www.youtube.com/watch?v=hb3rurFxrZ8)
- [Rich Hickey Playlist](https://www.youtube.com/playlist?list=PLZdCLR02grLrEwKaZv-5QbUzK0zGKOOcr)
- [Expert to Expert: Rich Hickey and Brian Beckman - Inside Clojure](https://www.youtube.com/watch?v=wASCH_gPnDw)
- [Every Clojure Talk Ever - Alex Engelberg and Derek Slager](https://www.youtube.com/watch?v=jlPaby7suOc)
- [Understanding Clojure's thread macros - Misophistful](https://www.youtube.com/watch?v=qxE5wDbt964)

## Community

I'd wager a guess that Clojure is the most popular of the Lisps out there, making it fairly easy to get into the community.

- [/r/clojure](https://old.reddit.com/r/Clojure/)
- [Clojure Discord (discljord)](https://discord.com/invite/discljord)
- [Clojure Google Group](https://groups.google.com/g/clojure)
- [Community Resources](https://clojure.org/community/resources)
- [Clojure Forum](https://ask.clojure.org/)
- [Clojurians Slack](http://clojurians.net/)

# Racket

[Racket](https://en.wikipedia.org/wiki/Racket_(programming_language)) is a dialect of Scheme that is used mainly as a tool for teaching, and as a playground for programming language research and development. It's often said that Racket has one of the most powerful macro systems amongst the Lisps - to these ends it's a great language. 

## Books

## Videos

## Community

## Conclusion