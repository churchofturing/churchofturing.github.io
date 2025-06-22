# Motivating Macros 

Lisps are peculiar. Trying to explain the appeal of a Lisp to someone is often an exercise in futility. Firstly because they are so strange and secondly because any explanation fails to capture the appeal in a way that's relatable to most programmers. 

The common approach is to mention the uniform syntax, or the joys of interactive development with a REPL (Read Eval Print Loop), or the sophisticated macro systems, or if you're feeling like totally losing your audience throwing out "homoiconicity". This description, while true, ultimately falls flat; I believe it's because these are things best experienced rather than described.

The point of this post is to touch on all of these topics from first principles using the Clojure programming language. Before we begin I also want to emphasise that this is a full-contact blog post and you wont get any benefit by just reading - you **have to** participate. Even if you're not convinced by the end, you still gave it a shot and that's what matters.

I also want to mention that there are very *many* books written on the topic at hand. It's quite a deep subject, and in the interest of keeping things digestable I will be moving fast and omitting a huge amount of detail. Hyperlinks are included for those who want to understand more.

# On REPLs

This section's a little bit of a detour, but I think it's important to contextualise what I mean by a REPL and interactive development. 

You may have used a Read Eval Print Loop (REPL) in other languages. For example, when I type `python` into my terminal this is what is returned.

```
❯ python
Python 3.13.3 (main, Apr 22 2025, 00:00:00) [GCC 15.0.1 20250418 (Red Hat 15.0.1-0)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Here is a Python REPL where I can enter Python code and have it read, evaluated and printed back to me. Neat!

However even in a language like Python that has a REPL, this isn't the primary way that people develop software using Python.

An simplified Python development loop is below:

1. Modify [file].py
2. Run `python [file].py` in the command line.
3. See output.
4. Go to step 1.

The REPL rarely makes an appearance beyond one-off experimentation. It's a place to try out a new library, play with a new idea or figure out how to call a function. 

Lisps (generally but not always) are different. Interactive REPL development is one of their strengths. A simplified development loop for Clojure is below.

1. Open your project in your editor of choice.
2. Start a REPL session and connect your editor to it.
3. Send expressions from your editor to the REPL to be evaluated.
4. Go to step 3.

The difference might not be immediately obvious, but it's very obvious in practice. The feedback loop from idea to result is drastically shortened. You can poke, prod and manipulate data in ways that would've been tedious otherwise. For long lived programs (such as GUIs or servers), you an change things as they're running and immediately see your changes reflected. It is something to be experienced.

If this has caught your interest and you want to learn more, I'd recommend watching the talk ["Stop Writing Dead Programs" by Jack Rusher](https://www.youtube.com/watch?v=8Ab3ArE8W3s).

# Setup

Did I mention you need to follow along? Because you do.

Broadly you'll need: 

1. Java (JDK)
2. [Clojure](https://clojure.org/guides/install_clojure)
3. An editor with Clojure REPL integration.

How you install Java and Clojure will depend on your operating system. I'm leaving it up to you to figure out this part.

Once you have Java and Clojure installed, you now need an [editor](https://clojure.org/guides/editors). There's a few different ways of editing Clojure. The most prominent ways are:

1. [VSCode](https://code.visualstudio.com/) and [Calva](https://calva.io/).
2. [IntelliJ and Cursive](https://cursive-ide.com/).
3. [Emacs](https://www.gnu.org/software/emacs/) and [CIDER](https://github.com/clojure-emacs/cider).

You'll notice these are all in the format of "[Editor] and [Plugin]". That's because to get the interactive REPL development mentioned earlier, there's a small bit of plumbing that needs to happen between your editor and Clojure. Trust me, it's worth it.

The easiest set up for a beginner in my opinion is VSCode + Calva. [Daniel Amber](https://www.youtube.com/@onthecodeagain) has a wonderful video showing how to set VSCode and Calva in only a few minutes. You can find it here, and I highly recommend you follow it:

- [Setup Clojure and run a REPL in VS Code Using Calva](https://www.youtube.com/watch?v=6uUynWkMDGM)

Once you have a running REPL and are able to send expressions to it from your editor, you're ready to move on.

There'll be quite a lot of code snippets ahead. For clarity, I will be including the result of an expression being evaluated underneath the expression with the prefix `=>`.

```clojure
(+ 2 3)
=> 5
```

This means I evaluated `(+ 2 3)` and the result was `5`. 

# Introduction to Clojure

Everything in Clojure is an expression. An expression can either evaluate to itself, or something else. 

## Expressions that evaluate to themselves

Try evaluating everything in this section.

### Numbers

```clojure
42
=> 42

1.61803
=> 1.61803

1/4
=> 1/4
```

### Strings

```clojure
"To be, or not to be, that is the question."
=> "To be, or not to be, that is the question."

"This
 spans
 multiple
 lines!"
=> "This\n spans\n multiple\n lines!"
```

### Keywords

Keywords are typically used either as keys in a map or as enums.

```clojure
:bird-is-the-word
=> :bird-is-the-word

:1337
=> :1337

:success
=> :success
```

### Booleans

```clojure
true
=> true

false
=> false
```

### Lists

In Clojure, a list is an ordered collection of elements, written inside parentheses. Lists are linked lists, which means they are efficient for adding andremoving elements at the front but slower for random access.


```clojure
'(1 2 3 4 5)
=> (1 2 3 4 5)

; They don't have to hold the same type.
'(false :1337 1/4 "U wot m8?")
=> (false :1337 1/4 "U wot m8?")

; This is another way to construct a list.
(list 5 4 3 2 1)
=> (5 4 3 2 1)
```

### Vectors

Vectors are ordered collections, like lists, except they have better performance characteristics for random access. They are closest to arrays in other programming languages. 

```clojure
[0 1 1 2 3 5 8]
=> [0 1 1 2 3 5 8]

(vector 0 1 1 2 3 5 8)
=> [0 1 1 2 3 5 8]
```


### Maps

Maps associate keys to values. If you're familiar with JavaScript, they're quite similar to objects. In other languages they may be called something else, such as hash maps or dictionaries. For now we'll just look at how to create maps, later on we'll look at how to manipulate and modify them. 

```clojure
{:name "Roland Deschain" :age 40}
=> {:name "Roland Deschain" :age 40}

; They can be nested too.
{:roland   {:name "Roland Deschain" :age 40}
 :eddie    {:name "Eddie Dean" :age 23}
 :susannah {:name "Susannah Dean" :age 26}
 :jake     {:name "Jake Chambers" :age 11}}

=> {:roland {:name "Roland Deschain", :age 40},
    :eddie {:name "Eddie Dean", :age 23},
    :susannah {:name "Susannah Dean", :age 26},
    :jake {:name "Jake Chambers", :age 11}}

; You can have vectors of maps.
[{:name "Red" :hex "#FF0000"}
 {:name "Green" :hex "#00FF00"}
 {:name "Blue" :hex "#0000FF"}
 {:name "Yellow" :hex "#FFFF00"}
 {:name "Black" :hex "#000000"}
 {:name "White" :hex "#FFFFFF"}]
=> [{:name "Red" :hex "#FF0000"}
    {:name "Green" :hex "#00FF00"}
    {:name "Blue" :hex "#0000FF"}
    {:name "Yellow" :hex "#FFFF00"}
    {:name "Black" :hex "#000000"}
    {:name "White" :hex "#FFFFFF"}]
```

### Sets

Sets are **unique** collections. That means they can't have multiple of the same element.

```clojure
#{"Scooby-Doo"
  "Shaggy Rogers"
  "Fred Jones"
  "Daphne Blake"
  "Velma Dinkley"}
=> #{"Velma Dinkley" "Daphne Blake" "Shaggy Rogers" "Scooby-Doo" "Fred Jones"}

#{:there :is :one :duplicate :keyword :duplicate}
; Syntax error reading source at (REPL:54:41).
; Duplicate key: :duplicate
```
## Expressions that evaluate to something else

Now that we've seen expressions that evaluate to themselves, what about expressions that evaluate to something else?

Broadly these come in two forms:

- **Lists** are evaluated as *invocations*. 
- **Symbols** can refer to something else. When evaluated, they return the thing they refer to. This is how variable names work.

The rest of this section will be spent mostly focusing on lists, and briefly touching on symbols. Clojure is a [list processor (Lisp)](https://en.wikipedia.org/wiki/Lisp_(programming_language)) after all. 

In Clojure, lists are a pretty core data structure. A list is written inside parenthesis, and contains zero or more elements. `(1 2 3)` is a list. So is `("tom" "dick" "harry")`. Also `(:hello "world" 42)`. 

However if you try to evaluate any of these lists in your editor, you'll get a strange and impossible to understand error that looks something like:

> ; Execution error (ClassCastException) at example.core/eval10217 (REPL:25).

> ; class java.lang.Long cannot be cast to class clojure.lang.IFn (java.lang.Long is in module java.base of loader 'bootstrap'; clojure.lang.IFn is in unnamed module of loader 'app')

This is because in Clojure lists have two purposes. One is to structure data as shown above, and the other is to invoke functions.

Try evaluating this list:

```clojure
(+ 1 2 3 4 5)
=> 15
```

When you evaluate a list in Clojure, it looks at the first element as the thing to invoke and passes it the rest of elements as arguments. In this example the `+` function is invoked with `1 2 3 4 5` as arguments, producing a result of 15. Functions aren't the only thing that can be invoked, but we'll come to that later.

Now what did that obscure error from earlier mean? All it was saying was that when you evaluate `(1 2 3 4 5)`, it tries to find a function `1` and give it the arguments `2 3 4 5`. There is no function called `1` so it doesn't know what to do.

The next logical question is "okay, if Clojure tries to invoke the first element of a list, how do you actually use lists to represent data?". The answer to that is: we quote the list.

```clojure
'(1 2 3 4 5)
=> (1 2 3 4 5)

; Can also be called like...
(quote (1 2 3 4 5))
=> (1 2 3 4 5)
```

That little quote symbol tells Clojure to just return the unevaluated form without trying to invoke it as a function. The former is just a shorthand version of the latter.

If you're particularly astute, you might notice that this allows you to treat things that would usually be invoked as static data. 

```clojure
(+ 1 2 3 4 5)
=> 15

'(+ 1 2 3 4 5)
=> (+ 1 2 3 4 5)
```

If we can treat the first expression as data instead of a function invocation, that means we can manipulate it like any other data structure. Tuck this in the back of your mind for now.

---

### Math

How do we do math in Clojure? It turns out very easily.

```clojure
; Note: a lot of the math functions can take any number of arguments.
(+ 1 2 3 4 5)
=> 15

(- 1000 50)
=> 950

(* 435 23345)
=> 10155075

(/ 100 2)
=> 50
```

The first symbols in the list are just functions. In a lot of other languages these math operations are implemented as special operators and have a specific order of precedence. In Clojure they're just functions like (almost) everything else, and the order they're evaluated is entirely unambiguous.

Expressions can be nested as well.
```clojure
(+ (* 2 5) (- 20 10))
=> 20

; Evaluation of sub-expressions happens from left to right.
; (+ (* 2 5) (- 20 10))
; (+ 10 (- 20 10))
; (+ 10 10)
; => 20
```

You can find more Clojure math functions [here](https://clojuredocs.org/clojure.math). They're really just wrapper calls to java.lang.Math.

```clojure
(Math/abs -1)
=> 1

(Math/pow 2 8)
=> 256.0

(Math/sqrt 998001)
=> 999.0

(Math/round 5.7)
=> 6

(Math/floor 5.7)
=> 5.0

(Math/ceil 5.1)
=> 6.0
```

### Do

This next bit is a little more subtle. If you come from a more traditional imperative-style language, you'll be used to having statements execute sequentially.

Take the JavaScript below.

```js
console.log("Three...");
console.log("Two...");
console.log("One...");
console.log("Blast off!");
return "success";
```

This is pretty straightforward to understand. Lines are executed top-to-bottom.

There's a common gotcha when people try to translate this idea directly to Clojure. You might arrive at something like:

```clojure
((println "Three...")
 (println "Two...")
 (println "One...")
 (println "Blast off!")
 "success")

; Three...
; Two...
; One...
; Blast off!
; Execution error (NullPointerException) at example.core/eval10310 (REPL:59).
; Cannot invoke "clojure.lang.IFn.invoke(Object, Object, Object, Object)" because the return value of "clojure.lang.IFn.invoke(Object)" is null
```

Why did it almost work, but blow up at the end? Earlier we saw that the evaluation of sub-expressions happens from left to right. The first (println ...) is being evaluated and returning nil, the second is being evaluated and returning nil and so on. This reduces to an expression that actually looks like `(nil nil nil nil "success")`. As we learned earlier, the leftmost element of the list will try to be invoked as a function, but given the leftmost is nil things fall apart!


So how actually do we translate the above JavaScript into the Clojure equivalet? The answer is by using `do`. The `do` special form evaluates multiple expressions in order, and returns the result of the last expression. 
```clojure
(do
  (println "Three...")
  (println "Two...")
  (println "One...")
  (println "Blast off!")
  "success")

; Three...
; Two...
; One...
; Blast off!
=> "success"
```

### Variables
---

Variables are defined using `def`. `def` creates a symbol, and binds the value to that symbol. Lets see some examples.

```clojure
(def meaning-of-life 42)
=> #'example.core/meaning-of-life

meaning-of-life
=> 42

(def simpsons-characters
  [{:name "Homer Simpson" :role "Father"}
   {:name "Marge Simpson" :role "Mother"}
   {:name "Bart Simpson" :role "Son"}
   {:name "Lisa Simpson" :role "Daughter"}
   {:name "Maggie Simpson" :role "Baby"}])
=> #'example.core/simpsons-characters

simpsons-characters
=> [{:name "Homer Simpson", :role "Father"}
    {:name "Marge Simpson", :role "Mother"}
    {:name "Bart Simpson", :role "Son"}
    {:name "Lisa Simpson", :role "Daughter"}
    {:name "Maggie Simpson", :role "Baby"}]
```

### Functions

Functions can be created a few different ways. Lets start with `fn`. `fn` creates an anonymous function (often called a lambda). All this means is it's a function with no name.

```clojure
(fn [x] (* x x))
=> #function[example.core/eval10280/fn--10281]
```
We can use this lambda in two ways: either directly, or by assigning it to a variable.

```clojure
; Directly (passes 10 as an argument to the leftmost function).
((fn [x] (* x x))
  10)
=> 100

; Assigning an anonymous function to a variable.
(def our-square (fn [x] (* x x)))
=> #'example.core/our-square

(our-square 10)
=> 100

; We can now check that these are equivalent.
(=
 (our-square 10)
 ((fn [x] (* x x))
  10))
=> true
```

Creating a lambda just to assign it to a variable can be tedious. This is why Clojure gives us `defn` (define function). `defn` creates a function and assigns it to a variable in one motion.

```clojure
(defn our-better-square
  [x]
  (* x x))

(=
  (our-square 23234)
  (our-better-square 23234))
=> true
```

Using just what we've learned so far, we can start to write interesting functions. Lets implement the [Euclidian distance](https://en.wikipedia.org/wiki/Euclidean_distance) function for 2D space.

The Euclidian distance function for 2D space is defined as:

```
distance = sqrt((p.x - q.x)^2 + (p.y - q.y)^2)
```

Here's how we would translate that into Clojure.

```clojure
(defn euclidian-distance
  [px py qx qy]
  (Math/sqrt
   (+
    (Math/pow (- px qx) 2)
    (Math/pow (- py qy) 2))))
=> #'example.core/euclidian-distance

(euclidian-distance 23 2 5 23)
=> 27.65863337187866
```

This part can be tricky for beginners, so be patient. Clojure is very explicit with the order the operations happen, so it forces you to think carefully how to translate it from normal mathematical syntax. 

### Control Flow

An important aspect of programming we've not yet touched on is control flow. Control flow can be broadly classified in two ways:

- Conditionals
- Iteration

Conditionals let your program make choices, and iteration allows your program to repeat actions without duplicating code. 

#### Conditionals

```clojure
(def player-age 19)
(if (> player-age 18)
  "Go on in!"
  "Come back in a while.")
=> "Go on in!"
```

Take a second and try to really understand what's happening here. The `if` takes a value, and if it's true returns the first expression or if it's false returns the second. This might prompt the question: "In Clojure, what is true?". This is quite easy to remember - it's everything that's not `nil` or `false`.

Lets look at another example of `if`. 

```clojure
(/ 5 0)
; Execution error (ArithmeticException) at example.core/eval9496 (REPL:15).
; Divide by zero

(if true
  (+ 10 10)
  (/ 5 0))
=> 20
```

I mentioned earlier that in Clojure expressions get evaluated left-to-right. Following this evlauation scheme, we might have expected the sub-expression `(+ 10 10)` to be evaluated to `20`, and then `(/ 5 0)` to be evaluated to an error. Why then didn't we get a divide by zero error? 

It's because the if-expression *never evalutes the second expression*. This goes against the standard evaluation rules of Clojure, and because of this `if` is called a [special form.](https://clojure.org/reference/special_forms) Later on we will see how we can write our own macros that changes the evaluation rules of Clojure. 

Moving on, `when` is just like `if` except with one branch. 

```clojure
(when (> 20 30)
  "I'll never be returned.")
=> nil

(when (> 20 10)
  "But I will be.")
=> "But I will be."
```

`cond` on the other hand allows multiple branches.

```clojure
(defn sign-checker
  [number]
  (cond
    (< number 0) :negative
    (> number 0) :positive
    :else :zero))
=> #'example.core/sign-checker

(sign-checker 20)
=> :positive

(sign-checker -3)
=> :negative

(sign-checker 0)
=> :zero
```


### Let Bindings

### Manipulating Data

### Map/Filter/Reduce

