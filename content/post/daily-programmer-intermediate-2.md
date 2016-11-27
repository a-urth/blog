+++
date = "2016-11-27T10:44:08+02:00"
title = "Daily programmer - saturday defusal"
tags = ["code", "programming", "dev", "algorithms", "clojure"]
+++

Right of the way gonna say that doing it on Saturday just because didn't feel like doing it at all. This task is based on Mondays one and consists of two parts. Task is [here](https://www.reddit.com/r/dailyprogrammer/comments/5emuuy/20161124_challenge_293_intermediate_defusing_the/), latest solution is [here](https://github.com/a-urth/daily-programmer/tree/master/20161124_intermediate).

So with regular task I came pretty quickly and there is really nothing special comparing with easy one.
```clojure
(def wire-rules {:s0 {\w :s1, \r :s2}
                 :s1 {\w :s2, \o :s3}
                 :s2 {\b :s3, \r :s0}
                 :s3 {\b :s3, \o :s4, \g :s5}
                 :s4 {\g :exit}
                 :s5 {\o :exit}})

(defn end? [kw] (= kw :exit))

(defn defuse
  ([wires]
   (defuse wires :s0))

  ([[wire & others] cur-step]
   (let [next-step ((wire-rules cur-step) wire)]
     (condp = next-step
       :exit true
       nil false
       (recur others next-step)))))

(defn defuse-bomb [i wires]
  (println
   (if (defuse (map first (split-lines wires)))
     (format "%s - Bomb defused" i)
     (format "%s - Booom" i))))
```
Rules are now represented is slightly different form, `defuse` function as well, but  its all pretty straightforward. The only thing I changed is implemented `condp` part in `defuse` function (saw it in other solution on reddit). Previously I had `if-let` and `if` which was not so good looking in my opinion.

While first part was pretty easy, there is bonus one, which is an interesting one. I've spent some time on solution design and came up with recursive algorithm with backtracking which looks something like this.
```clojure
(defn wires->map [wires]
  (into {} (for [wire-row (split-lines wires)
                 :let [[wire n] (split wire-row #" ")]]
             [(first wire) (Integer. n)])))

(defn no-wires-left? [wires-m] (every? zero? (vals wires-m)))

(defn defusable?
  ([wires-m]
   (defusable? wires-m :s0))

  ([wires-m step]
   (if (end? step)  ; (1)
     (no-wires-left? wires-m)  ; (2)
     (loop [[[wire next-step] & other] (seq (wire-rules step))]  ; (3)
       (if (nil? wire)
         false
         (let [wire-n (wires-m wire)]
           (if (and
                (not (zero? wire-n))  ; (4)
                (defusable? (assoc wires-m wire (dec wire-n)) next-step))  ; (5)
             true
             (recur other))))))))

(defn check-bomb [i wires]
  (println
   (if (defusable? (wires->map wires))
     (format "%s - Defusable" i)
     (format "%s - Not defusable" i))))
```
So I have two helper functions - one for converting input to map with wire colors as keys and number of wires as values (`wires->map`), and second for checking if there are still some wires left after finding correct solution (`no-wires-left?`).

The thing I don't like is how `defusable?` function looks like. General idea behind it is to go through rules step by step and check if we have correct number of wires to perform defusal. We gonna recursively check each branch and if at some point we'll find out that current branch is wrong we go back, which is backtracking part. Lets brake it in parts.
First we check if its a last step (__1__) and there are no wires left (__2__) then this is correct path and bomb is defusable. If there are still some wires left in the end, then this path is wrong and if its not the end yet - check current step. Checking current step means go through all possible branches for it (__3__) and check if we have enough wires to cut (__4__) and path down this branch satisfies rules (__5__). If both are true, then this path is correct, if not - check other ones.

Idea is pretty straightforward, implementation as well and it works as good as it can get. But i have a feeling that it could be written in better way, more idiomatic. I don't like so much nesting and it looks like that recursive `loop` can be changed with something more appropriate, but I have no clue with what exactly. I even asked for code review in clojure slack channel, and if I'll be lucky enough to get some clues where I'll do update of this solution.
