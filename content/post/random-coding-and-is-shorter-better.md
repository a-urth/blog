+++
date = "2016-11-28T12:32:27+02:00"
title = "Sunday night random coding and some thoughts on better programming"
tags = ["programming", "dev", "clojure", "python"]
+++

So it was Sunday late evening and I'm, almost sleeping, randomly cruising through reddit, came to this [article](https://www.new2code.com/2016/11/proof-you-can-become-a-better-programmer/). In which author shares his experience of becoming "better programmer", and I found it some what controversial for me. Generally he takes two common programming problems and compares how he did them in the start of his programming career and now. This is cool short reading itself and 100% educational, but in my opinion for another reason than author thought.

First one is great because it utilizes language features at its best. I'm not sure that its much more readable, but definitely shorter and, let me say, smarter way.

But second one is not so obvious and even controversial in my opinion, but let me explain myself. First of all this kind of programming task is quite common and I even wrote solutions by myself in `clojure`, both for problem author describes and more common one, as mention in comments, counting for consecutive characters, here they are.

Characters counting and shortening
```clojure
(defn chars-count [s]
  (fn reducer [acc c]
    (let [n (acc c)] (assoc acc c (if n (inc n) 1))))
  (clojure.string/join (for [[c n] (reduce reducer {} s)] (str c n))))
```

__Consecutive__  characters counting and shortening
```clojure
(defn compact [s]
  (loop [[char & rest] s
         last-char ""
         acc ""
         count 1]
    (if (nil? char)
      (str acc last-char count)
      (if (= last-char char)
        (recur rest char acc (inc count))
        (let [res (if (= last-char "") acc (str acc last-char count))]
          (recur rest char res 1))))))
```
As usual I'm not even trying to say that these are top notch solutions and there is no way to do it better, its just my version written being sleepy on Sunday late evening.

Now, why I don't like authors solution to second problem and why I'm thinking that its bad way to code. Yes, it again utilizes language features and is much shorter, but by what cost? Its absolutely not efficient and complexity of such solution is nowhere close to required one. Its pointed in comments couple of times and on [reddit](https://www.reddit.com/r/programming/comments/5f5rzg/proof_you_can_become_a_better_programmer_new_to/) thread there link to this article was posted. And the thing I disagree with the most is the point that such way is "better", when its actually not. Its shorter/cooler/smarter/you name it but not more efficient or even readable, actually in terms of efficiency its even worse than first one. Doing this way shows that You actually doesn't care about what and how Your code do, but care about how it looks instead. And author says that he would do it in other (correct) way if it would be some kind of interview with execution time or any other limitations which he cannot exceed. But the thing is that in real life You never now what are these limitations and when You'll exceed them. You write solution like that and Your application performs super good with no load and as customers start to visit Your web page (for example) CPUs in Your servers are melting because while You've been coding Your neat solution You didn't actually think that O(n) is way better than O(N^2) and this code will fuck up Your whole application.

And arguments like "in such little problem real efficiency doesn't matter, its just an example" are doing everything even worser. Because by doing such tasks You gaining programming habits and skills, and if You don't care about how Your code performs than You'll never be "better programmer".

This case reminded me [this task](https://www.codewars.com/kata/most-frequent-elements/solutions/python) which is almost the same and even most voted solution is similar.
```python
def find_most_frequent(l):
    return set(x for x in set(l) if l.count(x) == max([l.count(y) for y in l]))
```

Its just ridiculous how this thing is slow on bigger inputs and no wonder because its O(N^3) since `count` is O(N) and its done for each element of the input list on each iteration... This is actually great example of how You should __NOT__ write code. You can compare it with other solutions on that page which utilize regular python's `dict` and even better ones with `Counter` which are actually smart and pythonic in my opinion.

So, all I wanted to say is that shorter solutions are not always "better" or even more readable ones. Never compromising performance of Your code and still finding way to make it look great, understanding and realizing __how__ Your code works and __what__ it actually does on each step - this is true meaning of being "better programmer" in my opinion.
