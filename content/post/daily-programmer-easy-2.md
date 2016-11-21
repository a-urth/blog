+++
date = "2016-11-21T23:35:07+02:00"
title = "Daily programmer (easy Monday)"
tags = ["programming", "dev", "code", "algorithms", "dailyprogrammer", "clojure"]
+++
Another easy Monday except this one is little bit more easy than last one. [Here](https://www.reddit.com/r/dailyprogrammer/comments/5e4mde/20161121_challenge_293_easy_defusing_the_bomb/) is the task.

I finished rather fast with initial version, but doing little changes for of times I've ended up with this.
```clojure
(ns daily-programmer.20161121_easy.core
  (:use [clojure.string :only [split-lines]]))

(def wire-rules-m {"white" {:to-cut #{} :not-to-cut #{"white" "black"}}
                   "red" {:to-cut #{"green"} :not-to-cut #{}}
                   "black" {:to-cut #{} :not-to-cut #{"white" "green" "orange"}}
                   "orange" {:to-cut #{"red" "black"} :not-to-cut #{}}
                   "green" {:to-cut #{"orange" "white"} :not-to-cut #{}}
                   "purple" {:to-cut #{} :not-to-cut #{"purple" "green" "orange" "white"}}})

(defn can-cut? [wire & [{:keys [to-cut not-to-cut]} rules]]
  (and
   (or (empty? to-cut) (contains? to-cut wire))
   (or (empty? not-to-cut) (not (contains? not-to-cut wire)))))

(defn defuse
  ([wires]
   (defuse wires {:to-cut #{}, :not-to-cut #{}}))

  ([[wire & others] rules]
   (let [cut (can-cut? wire rules)]
     (if (nil? others)
       cut
       (and cut (recur others (get wire-rules-m wire)))))))

(println (if (defuse (split-lines (slurp *in*))) "Bomb defused" "Boom"))

```
And as You can guess I didn't have a clue how to implement real state machines. If You want to see something cool and fast then You should definitely check couple of top comments on reddit thread, its insane!

So, getting back to my solution - major things I changed comparing to initial version are:
* changed `loop` and default arguments in it to function with two arities and doing recur directly to it;
* replaced `if` form with `and` so it looks shorter and cleaner e.g.
```clojure
(and cut (recur others (get wire-rules-m wire))
```
* previously rules map destructuring was done in `let` just before recursion call, moved it to `can-cut?` function arguments;
* changed names of rules map keys, now its much better then "allowed" and "disallowed" in my opinion.
