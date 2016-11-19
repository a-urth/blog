+++
title = "Saturday morning - random coding problems solving"
date = "2016-11-19T11:31:39+02:00"
tags = ["clojure", "dev", "programming", "code"]
+++

So I just randomly (from [/r/coding](http://reddit.com/r/coding) actually) came across this [site](http://www.gainlo.co/) which is built over idea of mocking interviews similar to big IT companies do. And it got my attention because it has some examples of programming tasks in its blog. Tasks, which I found seem to be not hard at all (authors mention this as well) and I think that such problems may be asked either for lower grade positions or during initial stages of interviewing process.

But anyway, I found some of them really interesting and since I had some time this Saturday morning I thought - way not to sharpen my coding skills with beautiful clojure (because being python dev usually on interviews I'm doing them in python).

##### [Move Zeroes](http://blog.gainlo.co/index.php/2016/11/18/uber-interview-question-move-zeroes/)
> Modify the array by moving all the zeros to the end (right side). The order of other elements doesn’t matter.

I came with this straightforward solution. I know that it can be done better and in fact, later I did it in python in similar way which described in blog post. But this one sees to be neat and tiny.

```clojure
(def problem [1 2 0 3 0 1 2])

(defn reducer [[acc i] e]
  (if (zero? e)
    [acc (inc i)]
    [(conj acc e) i]))

(let [[non-zeros zeros-num] (reduce reducer [[] 0] problem)]
  (into non-zeros (repeat zeros-num 0)))
```

So I'm just going through list of values counting how much zeroes there are and filtering result for them. After that I'm just adding required number of zeroes to filtered values.

In the blog post You can find better solution which requires data mutation, I think it can be written in `clojure` as well but I just wasn't interested in that.

##### [Delimiter Matching](http://blog.gainlo.co/index.php/2016/09/30/uber-interview-question-delimiter-matching/)
> Write an algorithm to determine if all of the delimiters in an expression are matched and closed.

This is pretty popular coding task and I'm pretty sure that everyone faced it in one way or another if they ever done some programming stuff on data structures. Because this is the most popular showcase for `stack`.

```clojure
(def problem "[dklf(df(kl))d]{")

(def brackets-m {\] \[, \} \{, \) \(})

(def closings (set (keys brackets-m)))

(def openings (set (vals brackets-m)))

(def error-msg "Delimiters unmatched!")

(defn match? [o c] (= (get brackets-m c) o))

(defn process-sym [res s]
  (cond
    (contains? openings s) (conj res s)
    (contains? closings s) (if (match? (last res) s)
                             (drop-last res)
                             (throw (Exception. error-msg)))
    :else res))

(let [s (reduce process-sym [] problem)]
  (when (not (empty? s))
    (throw (Exception. error-msg))))

```

There are plenty of `def`s and helper function's declaration but main idea is in `process-sym` function. What id does is checks if we have other than bracket symbol - just continue, if its an opening bracket - put it into stack, if closing one - take last from stack and compare them. If they do not match each other - raise exception which means that delimiters are not matched. Same check is required after processing whole sequence - if there is still something in the stack, then raise exception. `nil` returned if delimiters are matched. I did it with exceptions just because its easier to propagate error in `process-sym` but it can be approached in some other ways.

##### [Longest Substring Without Repeating Characters](http://blog.gainlo.co/index.php/2016/10/07/facebook-interview-longest-substring-without-repeating-characters/)
> Given a string, find the longest substring without repeating characters.

I'm sure that this one is also popular programming question but I actually never came across it. Right of the way I've noticed naive solution (check all substrings), but its obvious that this was not the most efficient one, so I've decided to think a little before writing even single character. Soon I found that we can store symbols in set to check for repeats and use two pointers for current substring and in case of repeat we should start next search from position of repeated symbol. But my mistake was that I wanted to use position of second repeated symbol instead of first one. In order to do that efficient I changed my set to map with symbols as keys and their indexes as values and it worked like a charm.

```clojure
(def problem "abccdefdgh")

(def problem-l (count problem))

(loop [start-i 0 end-i 0
       cur-syms-m {}]
  (if (= end-i problem-l)
    (subs problem start-i end-i)
    (let [cur-sym (get problem end-i)
          cur-i (get cur-syms-m cur-sym)
          next-end-i (inc end-i)]
      (if (nil? cur-i)
        (recur start-i next-end-i (conj cur-syms-m {cur-sym end-i}))
        (recur (inc cur-i) next-end-i {cur-sym cur-i})))))
```
In blogpost You can find solution in python which uses similar but slightly different approach.

##### [Design Hit Counter](http://blog.gainlo.co/index.php/2016/09/12/dropbox-interview-design-hit-counter/)

Interesting question on systems design, again, pretty common one. Only thing that I would like to add is if You need to count only unique visitors and we are talking about really big amount of data then speed and memory efficiency are much more important then exact answer, so approximation approaches such as [HyperLoglog](https://en.wikipedia.org/wiki/HyperLogLog) should be mentioned.
