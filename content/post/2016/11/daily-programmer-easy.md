+++
date = "2016-11-17T18:05:25+02:00"
title = "Daily programmer (easy Monday)"
tags = ["programming", "dev", "code", "algorithms", "dailyprogrammer", "clojure"]
+++
Since this is first post on daily-programmer topic I need some introduction.
So there is this awesome sub reddit called [DailyProgrammer](http://reddit.com/r/dailyprogrammer/) where programming tasks on algorithms are posted 3 times a week. Easy ones on Mondays, intermedium ones on Tuesdays and hard ones on Friday. You can post Your solutions in favorite language and compile and test them directly in the comment section with `/u/CompileBot`.

I've been aware about this thing for already quite some time but never was able to stick with it for too long. Actually my last try lasted only for one easy task, which is hilarious, I know.

And as You've already may guessed, since I'm some what trying to get closer to [Clojure](http://clojure.org), recently I decided to do one more try with daily-programmer. Here is my solution to latest task at this moment (its description can be found [here](https://www.reddit.com/r/dailyprogrammer/comments/5d1l7v/20161115_challenge_292_easy_increasing_range/)), I'll go through main points and my design decisions, mainly to do small repeat to myself and maybe improve something.


Here is my solution and lets break it down into blocks
```Clojure
(use '[clojure.string :only [split split-lines join]])

(defn pow10 [i] (reduce * (repeat i 10)))

(defn unshorten [x y]  ; (4)
  (if (> y x)
    y
    (let [l-y (count (str y))
          pred-x (quot x (pow10 l-y))  ; (4.1)
          last-x (mod x (pow10 l-y))  ; (4.2)
          pred (if (< last-x y) pred-x (inc pred-x))]  ; (4.3)
      (Integer. (str pred y)))))  ; (4.4)

(defn range-part [acc s]  ; (3)
  (let [[t-start t-end t-step] (map #(Integer. %) (split s #"-|:|\.{2}"))
        n (last acc)
        start (if (nil? n) t-start (unshorten n t-start))  ; (3.1)
        end (inc (if (nil? t-end) start (unshorten start t-end)))
        step (if (nil? t-step) 1 t-step)]
    (into acc (range start end step))))  ; (3.2)

(defn parse-range [s]
  (println (join " " (reduce range-part [] (split s #",")))))  ; (2)

(seq (map parse-range (split-lines (slurp *in*))))  ; (1)
```

##### (1)
First we read input from stdin - its just the most convenient way to test/run and test after posting. Split input in lines and map our parse function over them. `seq` function is called because `map` is lazy and without creating sequence it well return nothing since parse won't be called even once.

##### (2)
Then we split each line in smaller parts separated by comma, which will represent each individual range. And reduce these parts with function responsible for creating sequences. Then join results into one string separated with spaces and print it to stdout.

##### (3)
`range-part` function which generates sequences and adds to result accumulator only does performs preparation and utilizes built-in `range` function. Data preparation consists of splitting each part into `start`, `end` and `step` components and taking last value from result so we can know if current ones are in short form or not. Since some of components may be missing (except start of course) we check if they present and `unshorten` them (3.1). After having all these parts we just create sequence and adding it to result vector (3.2).

##### (4)
Everything before that was pretty easy and straightforward for me right of the way. But that `unshorten` thing really got me scratching my head, because right from the start I could tell that there is way to do that in constant time, but couldn't actually get it. And on reddit is posted older solution with this operation hella not optimized. This is actually the reason for me to write it down - analyze everything and find better solution. So here it is in some details.
```clojure
(defn unshorten [x y]  ; (4)
  (if (> y x)
    y
    (let [l-y (count (str y))
          pred-x (quot x (pow10 l-y))  ; (4.1)
          last-x (mod x (pow10 l-y))  ; (4.2)
          pred (if (< last-x y) pred-x (inc pred-x))]  ; (4.3)
      (Integer. (str pred y)))))  ; (4.4)
```
First of all after converting numbers to integers I check if current number actually smaller than previous, because if not I just return it. Then I split previous number (`x`) in two parts: one is right part with same length as current number (4.2) and second one is basically everything that comes to the left (4.1). After that split I need to decide what to add to the current number on the left side (since we now that right side will be our current number) and it either will be left part of previous number (if right side is smaller than current) as it is or it will be increased by one (4.3). And having those two parts of result I just concat them and convert back to integer (4.4).

So this was a little break down of solution which I came up with, and since this Wednesday there was no intermedium task I had time to do it and analyze better.

All sources for daily-programmer solutions I did can be found on [github](https://github.com/a-urth/daily-programmer)
