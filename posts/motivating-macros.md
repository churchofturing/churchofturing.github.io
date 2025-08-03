# Motivating Macros 

> The Tao that can be told is not the eternal Tao. - Lao Tzu

Lisps are peculiar. I've come to realise that trying to explain the appeal of a Lisp to someone is an exercise in futility. Firstly because Lisps are strange and secondly because any explanation fails to capture the appeal in a way that's relatable to most programmers. 

The common approach is to mention the uniform syntax, or the joys of interactive development with a REPL (Read Eval Print Loop), or the sophisticated macro systems, or if you're feeling like totally losing your audience throwing out "homoiconicity". This description, while true, ultimately falls flat; I believe it's because these are things best experienced rather than described.

The point of this post is to touch on all of these topics from first principles using the Clojure programming language. Before we begin I also want to emphasise that this is a full-contact blog post and you wont get any benefit by just reading - you **have to** participate. Intuition is formed through experience.

I also want to mention that there are very *many* books written on the topic at hand. It's a deep subject, and in the interest of keeping things digestable I will be moving fast and omitting a huge amount of detail. Hyperlinks are included for those who want to understand more.

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
3. Send expressions directly from your editor to the REPL to be evaluated.
4. Go to step 3.

The difference might not be immediately obvious, but it's very obvious in practice. The feedback loop from idea to result is drastically shortened. You can poke, prod and manipulate data in ways that would've been tedious otherwise. For long lived programs (such as GUIs or servers), you an change things as they're running and immediately see your changes reflected. It is something to be experienced.

If this has caught your interest and you want to learn more, I'd recommend watching the talk: ["Stop Writing Dead Programs" by Jack Rusher](https://www.youtube.com/watch?v=8Ab3ArE8W3s).

# Setup

Did I mention you need to follow along?

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

Everything in Clojure is an expression. An expression can either evaluate to itself or something else. 

## Expressions that evaluate to themselves

Try evaluating everything in this section, even if it's not very exciting. 

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

In Clojure, a list is an ordered collection of elements written inside parentheses. Lists are linked lists.


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

Vectors are ordered collections like lists, except they have better performance characteristics for random access. They are closer to arrays in other programming languages. 

```clojure
[0 1 1 2 3 5 8]
=> [0 1 1 2 3 5 8]

(vector 0 1 1 2 3 5 8)
=> [0 1 1 2 3 5 8]
```


### Maps

Maps associate keys to values. If you're familiar with JavaScript, they're quite similar to objects. In other languages they may be called something else such as hash maps or dictionaries. For now we'll just look at how to create maps, later on we'll look at how to manipulate and modify them. 

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

- **Lists** that are evaluated as *invocations*. 
- **Symbols** that refer to something else. When evaluated, they return the thing they refer to. This is how variable names work.

The rest of this section will be spent mostly focusing on lists, and briefly touching on symbols. Clojure is a [list processor (Lisp)](https://en.wikipedia.org/wiki/Lisp_(programming_language)) after all. 

In Clojure, lists are a pretty core data structure. A list is written inside parenthesis, and contains zero or more elements. `(1 2 3)` is a list. So is `("tom" "dick" "harry")`. Also `(:hello "world" 42)`. 

However if you try to evaluate any of these lists in your editor, you'll get an impossible to understand error that looks something like:

> ; Execution error (ClassCastException) at example.core/eval10217 (REPL:25).

> ; class java.lang.Long cannot be cast to class clojure.lang.IFn (java.lang.Long is in module java.base of loader 'bootstrap'; clojure.lang.IFn is in unnamed module of loader 'app')

This is because in Clojure lists have two purposes. One is to structure data as shown above, and the other is to invoke functions.

Try evaluating this list:

```clojure
+
=> #function[clojure.core/+]

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
; Note: a lot of the math functions can take a variable number of arguments. The fancy word for this is "variadic". 
(+ 1 2 3 4 5)
=> 15

(- 1000 50)
=> 950

(* 435 23345)
=> 10155075

(/ 100 2)
=> 50
```

The first symbols in the list are just functions. In a lot of other languages these math operations are implemented as special operators, and have a specific order of precedence. In Clojure they're just functions like (almost) everything else, and the order they're evaluated is entirely unambiguous.

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

Side note: this structure where operators precede their operands is known as [Polish notation](https://en.wikipedia.org/wiki/Polish_notation). At first this can look incredibly unintuitive as most of us have spent our lives internalising how to read [infix](https://en.wikipedia.org/wiki/Infix_notation) expressions and the order in which to evaluate them. There is however an advantage to this uniform structure, even if it's not immediately obvious: it very much simplifies metaprogramming. Tuck that away for now, but it's something we're going to be leveraging later.

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

Take the JavaScript below:

```js
console.log("Three...");
console.log("Two...");
console.log("One...");
console.log("Blast off!");
return "success";
```

This is pretty straightforward to understand, lines are executed top-to-bottom.

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

Functions can be created a few different ways. Lets start with `fn`. `fn` creates an anonymous function (often called a lambda). This just means is it's a function with no name.

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

; Naming the nameless - assigning an anonymous function to a variable.
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

Here's how we would translate that into Clojure:

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

Translating infix math expressions into expressions can be awkward for people not accustom to it, so be patient. Clojure is very explicit with the order of operations.

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

Take a second and try to really understand what's happening here. The `if` takes a value, and if it's true evaluates the first expression or if it's false evaluates the second. This might prompt the question: "In Clojure, what is true?". This is quite easy to remember - it's everything that's not `nil` or `false`.

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

I mentioned earlier that in Clojure expressions get evaluated left-to-right. Following this evlauation scheme, we might have expected the sub-expression `(+ 10 10)` to be evaluated to `20`, and then `(/ 5 0)` to be evaluated to an error. Then why didn't we get a divide by zero error? 

It's because the if-expression *never evalutes the second expression*. This goes against the standard evaluation rules of Clojure, and because of this `if` is called a [special form.](https://clojure.org/reference/special_forms) Later on we will see how we can write our own macros that changes the evaluation rules of Clojure. 

One last thing on `if`. You might wonder something like "Given `if` only takes two expressions, how do we perform multiple actions in a branch?". This is where `do` comes in handy.

```clojure
(if (> 10 5)
  (do
    (println "10 is larger than 5.")
    (println "...to the surprise of nobody.")
    :greater)
  (do
    (println "10 is less than or equal to 5.")
    (println "...to the surprise of everybody.")
    :less-than-or-equal))
; 10 is larger than 5.
; ...to the surprise of nobody.
=> :greater
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

In other programming languages, it's normal to create variables throughout a function's lifetime.

```js
function calculateInvoice(totalItems, pricePerItem, taxRate) {
  const subtotal = totalItems * pricePerItem;
  const tax = subtotal * taxRate;
  const total = subtotal + tax;
  return {
    subtotal,
    tax,
    total,
  };
}

calculateInvoice(10, 50, 0.06)
=> {subtotal: 500, tax: 30, total: 530}
```

To achieve something similar in Clojure, we're going to use the [let special form](https://clojuredocs.org/clojure.core/let). `let` is just a way to bind data to symbols, and in effect create variables that are local to the expression. 

Lets see some examples.

```clojure
(let [hello "world"]
  hello)
=> "world"

hello
; Syntax error compiling at (c:\...\core.clj:0:0).
; Unable to resolve symbol: hello in this context
```

Let takes pairs. The first item is the binding to create, and the second is the data to assign it. Here we're just saying "bind `hello` to `world`", which you can then reference anywhere inside the body of the let. If you try to evaluate `hello` outside of the let binding you can see that an error is thrown.

Lets translate the JavaScript function to Clojure using `let`.

```clojure
(defn calculate-invoice [total-items price-per-item tax-rate]
  (let [subtotal (* total-items price-per-item)
        tax (* subtotal tax-rate)
        total (+ subtotal tax)]
    {:subtotal subtotal
     :tax tax
     :total total}))

(calculate-invoice 10 50 0.06)
=> {:subtotal 500, :tax 30.0, :total 530.0}
```

Hopefully you can infer from context what's happening here. 


#### Iteration

Clojure has a few different ways of doing iteration.

`dotimes` is quite similar to for-loops in other languages.

```clojure
(dotimes [i 10]
  (if (even? i)
    (println (str i " is even"))
    (println (str i " is odd"))))
; 0 is even
; 1 is odd
; 2 is even
; 3 is odd
; 4 is even
; 5 is odd
; 6 is even
; 7 is odd
; 8 is even
; 9 is odd
=> nil
```

`doseq` iterates over a sequence, and binds each individual element to a variable.

```clojure
(def simpsons-characters
  [{:name "Homer Simpson" :role "Father"}
   {:name "Marge Simpson" :role "Mother"}
   {:name "Bart Simpson" :role "Son"}
   {:name "Lisa Simpson" :role "Daughter"}
   {:name "Maggie Simpson" :role "Baby"}])
=> #'example.core/simpsons-characters

(doseq [character simpsons-characters]
  (println (get-in character [:role])))
; Father
; Mother
; Son
; Daughter
; Baby
=> nil
```

If you were to translate the above into English it would be something like: *for every character in the array of Simpsons characters, print their role in the family.*


### Manipulating Data

Earlier we looked at how to create data structures, now lets look at how to actually do something with them.

```clojure
(def programming-languages ["C" "C++" "Clojure" "Python" "Rust" "Go" "JavaScript"])
=> #'namespace.core/programming-languages

(first programming-languages)
=> "C"

(second programming-languages)
=> "C++"

; 0 indexed
(nth programming-languages 2)
=> "Clojure"

(nth programming-languages 20)
; Execution error (IndexOutOfBoundsException) at playground.core/eval9554 (REPL:76).
; null

; You can also supply a value that will be returned if the nth index does not exist
(nth programming-languages 20 :not-found)
=> :not-found

; We can also add values to a collection using conj
(conj programming-languages "Ruby")
=> ["C" "C++" "Clojure" "Python" "Rust" "Go" "JavaScript" "Ruby"]

; But you'll notice that this doesn't update programming-languages.
; Conj doesn't update state. You're expected to use the result to create a new variable.
; Or update the value of an atom, which I wont get into.
; https://ericnormand.me/mini-guide/clojure-atom
programming-languages
=> ["C" "C++" "Clojure" "Python" "Rust" "Go" "JavaScript"]
```

These functions also work on lists too. In fact, they work on any data structure that implements the sequence interface.

```clojure

(def sicp-characters '("Eva Lu Ator" "Ben Bitdiddle" "Alyssa P. Hacker"))
=> #'namespace.core/sicp-characters

(first sicp-characters)
=> "Eva Lu Ator"

(second sicp-characters)
=> "Ben Bitdiddle"

(nth sicp-characters 2)
=> "Alyssa P. Hacker"

; Anything that can return a sequence can be treated as one - even strings.
(first (nth sicp-characters 2))
=> \A
```

Lets look at maps now.

```clojure
(def inventor-of-lisp
  {:name "John McCarthy"
   :field "Artificial Intelligence"
   :known-for [{:achievement "Inventing Lisp"}
               {:achievement "Coining 'AI' Term"}]
   :born 1927})
=> #'namespace.core/inventor-of-lisp

; Get the value associated with the :name keyword
(get inventor-of-lisp :name)
=> "John McCarthy"

; Keywords can also act as accessor functions
(:name inventor-of-lisp)
=> "John McCarthy"

; So can the map itself
(inventor-of-lisp :name)
=> "John McCarthy"


; We can also use get-in
(get-in inventor-of-lisp [:name])
=> "John McCarthy"

; get-in is particularly useful for nested data
(get-in inventor-of-lisp [])
=> {:name "John McCarthy",
    :field "Artificial Intelligence",
    :known-for [{:achievement "Inventing Lisp"} {:achievement "Coining 'AI' Term"}],
    :born 1927}

(get-in inventor-of-lisp [:known-for])
=> [{:achievement "Inventing Lisp"} {:achievement "Coining 'AI' Term"}]

(get-in inventor-of-lisp [:known-for 0])
=> {:achievement "Inventing Lisp"}

(get-in inventor-of-lisp [:known-for 0 :achievement])
=> "Inventing Lisp"

; We can also add to a map using assoc
(assoc inventor-of-lisp :death 2011)
=> {:name "John McCarthy",
    :field "Artificial Intelligence",
    :known-for [{:achievement "Inventing Lisp"} {:achievement "Coining 'AI' Term"}],
    :born 1927,
    :death 2011}

; Similarly to conj, this doesn't actually mutate the variable
inventor-of-lisp
=> {:name "John McCarthy",
    :field "Artificial Intelligence",
    :known-for [{:achievement "Inventing Lisp"} {:achievement "Coining 'AI' Term"}],
    :born 1927}
```

We've looked at how to manipulate data by retrieving and adding elements, but we've left out the very important idea of *data transformation*.

For collections, we're going to briefly look at how we can transform them using `map`, `filter` and `reduce`. 


#### Map

All `map` does is iterate over a collection and apply a function to each individual element, then returns this new collection of transformed elements.

```clojure
; This function takes a number as an argument and adds one to it 
(defn increment [number] (+ number 1))
=> #'namespace.core/increment

; We can see that every element has had `increment` applied to it.
(map increment '(0 1 2 3 4))
=> (1 2 3 4 5)

; Above is equivalent to:
(map (fn [x] (+ x 1)) '(0 1 2 3 4))
=> (1 2 3 4 5)

; Which is also equivalent to using the lambda shorthand syntax:
(map #(+ % 1) '(0 1 2 3 4))
=> (1 2 3 4 5)

(clojure.string/upper-case "Hello world")
=> "HELLO WORLD"

(map
 clojure.string/upper-case
 '("Homer" "Maggie" "Lisa" "Marge" "Bart"))
=> ("HOMER" "MAGGIE" "LISA" "MARGE" "BART")

; You can also pass multiple sequences to map.
; It applies the function to the first elements of each, then the second elements of each etc.
(map * '(1 2 3 4 5) '(10 20 30 40 50))
=> (10 40 90 160 250)

(map + '(1 2 3 4 5) '(5 4 3 2 1))
=> (6 6 6 6 6)

; We can even use it to transform maps (the datastructure, not the function).
(def item-stock {:ps4 3 :ps5 23 :switch2 14 :xbox-one 2})
=> #'namespace.core/item-stock

(defn double-stock
  [[key value]]
  [key (* value 2)])
=> #'namespace.core/double-stock

(map double-stock item-stock)
=> ([:ps4 6] [:ps5 46] [:switch2 28] [:xbox-one 4])

; Convert it back into a Map
(into 
 {}
 (map double-stock item-stock))
=> {:ps4 6, :ps5 46, :switch2 28, :xbox-one 4}
```

#### Filter

Filter, as the name suggests, filters items from a sequence by applying a predicate function (a function that returns true or false) to each element of the sequence and keeping those that return true. 

```clojure
(even? 1)
=> false

(even? 2)
=> true

(even? 3)
=> false

(filter even? '(1 2 3 4 5 6 7 8 9 10))
=> (2 4 6 8 10)


(count "hello world")
=> 11

(def welcome-messages '("Hi" "Hello" "Welcome" "Hi how are you?"))

; Only keep welcome messages that are less than 10 characters
(filter
 (fn [elem]
   (< (count elem) 10))
 welcome-messages)

=> ("Hi" "Hello" "Welcome")

(def item-stock {:ps4 6, :ps5 46, :switch2 28, :xbox-one 4})

; Only keep stock with more than 5
(filter
 (fn [[_ value]]
   (> value 5))
 item-stock)

; Convert it back to a Map
(into
 {}
 (filter
  (fn [[_ value]]
    (> value 5))
  item-stock))
```

#### Reduce

## Metaprogramming

I'm just going to quote Wikipedia for this one:

> Metaprogramming is a computer programming technique in which computer programs have the ability to treat other programs as their data. It means that a program can be designed to read, generate, analyse, or transform other programs, and even modify itself, while running.

In short it's a fancy way of saying "programs that write other programs". Macros are just pieces of Lisp that operate on other pieces of Lisp - thus facilitating metaprogramming. Before we get into Clojure macros I'm going to take a quick detour into metaprogramming with JavaScript.

### A JavaScript Detour

JavaScript has this neat function called [eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval). Eval takes a string representation of some JavaScript code and, as the name suggests, evaluates it. It turns out you can do some pretty cool things with just eval. 

Note: I'm using NodeJS - if you try to run these examples in your browser you may get security related errors as running eval on user input is a very bad idea. 

```js
eval("1 + 1");
=> 2

eval("console.log('printing from inside eval');");
// printing from inside eval
=> undefined

const program = "x => x * 2";

eval(program);
=> [Function (anonymous)]

const double = eval(program);

double(10);
=> 20
```

Not only can we represent programs in using strings, we can also manipulate and modify them.

```js
const double = x => x * 2;

double(5);
=> 10

console.log(double.toString());
// x => x * 2
=> undefined


const tripleProgram = double.toString().replace("2", "3");
console.log(tripleProgram);
// x => x * 3
=> undefined

const triple = eval(tripleProgram);
triple(5);
=> 15
```

So in JavaScript we can represent programs as data, and change that data programmatically. There's two big issues with this though:

- Manipulating structured data (code) as unstructured text is very awkward for anything more complex than find/replace.
- There's no way to use this technique to extend JavaScript itself. 

To highlight the second point, let me walk through a contrived scenario.

Imagine you've been programming in Ruby a lot recently, and you've fallen deeply in love with its `unless` statement.

```ruby
weather = "rainy"
puts "You can't play outside" unless weather == "sunny"
# "You can't play outside"
```

Now you're programming mainly in JavaScript, but in the back of your head you really wish you had something similar. You begin to imagine a construct like:

```js
const weather = "rainy";
unless (weather === "sunny" {
  console.log("You can't play outside");
}
```

You begin to imagine the process of this getting this added to JavaScript.

- Well we would need a proposal explaining the syntax/semantics/motivation around the addition.
- Then we would need to bring the proposal to the [TC39](https://tc39.es/) group and go through the 6 [proposal stages](https://tc39.es/process-document/).
- If we managed to make it through this process then the real fun begins - figuring out how to get it implemented in the prominent JavaScript engines and how to update tooling to support it.
- Maybe with a couple of miracles we would be able to use `unless` in 2-10 business years.

If you're a reasonable person you'd probably start to realise how unfeasible this is, given the time and effort required for such a relatively small addition.

What about writing it in JavaScript?

```js
const weather = "rainy";
const unless = (condition, branch) => { if (!condition) branch(); };

unless(weather === "sunny", () => console.log("You can't play outside"));
// "You can't play outside
=> undefined
```

Well, it *works*, but it doesn't really feel like a part of JavaScript. There's also the very small overhead of the extra function call. 

You might start to toy about with the idea of writing a superset of JavaScript - a transpiler called UnlessScript with the one singular addition of analysing files for your `unless` construct and translating it to JavaScript's native `if` and `else`. You spend an afternoon trying to write a parser for JS and you rename your files to *.us, but it quickly becomes obvious how much of a headache this is too. Lets abandon this thought for now - nothing gained and nothing lost.

### Clojure Macros

I can't believe I took this long to get to the point, but here we are. 

Remember from earlier that lists have a double life in Clojure: to represent data and to invoke functions?
This symmetry turns out to be very useful. 

```clojure
; We can evaluate the addition of 5 numbers
(+ 1 2 3 4 5)
=> 15

; We can also delay the evaluation using quote
'(+ 1 2 3 4 5)
=> (+ 1 2 3 4 5)

; Give us the first element in the list
(first '(+ 1 2 3 4 5))
=> +

; Now give us the arguments
(rest '(+ 1 2 3 4 5))
=> (1 2 3 4 5)

; Add the multiplication function to the start of the list of arguments
(conj (rest '(+ 1 2 3 4 5)) '*)
=> (* 1 2 3 4 5)

; Clojure has eval too, except it doesn't take a string
(eval (conj (rest '(+ 1 2 3 4 5)) '*))
=> 120
```
