+++
date = "2016-11-25T14:35:40+02:00"
title = "Weekly feature - second and last for November 2016"
tags = ["dev", "weeklyfeature", "cap", "python", "oop", "go", "rust"]
+++
Again I'm bringing pile of articles I've read this week and found them interesting enough to share.

The most interesting things in my opinion were those tensioned discussions all over the internet about Zed Show's article on python 3. Also I've read some materials on CAP theorem, yes, again, to clarify missing moments for me one more time. And I finally found book on Clojure for myself. I twice dismissed `Brave and true`, its not bad in any sense, but just doesn't fit my needs. `Programming clojure` on the other hand appeared to be very interesting and informative reading. In couple of weeks I hope I'll do a review for that.

#### Articles on software development stuff
* Interesting bug related with hash tables in `rust` which made lookup/insert operations go quadratic and interesting discussion also. [https://www.reddit.com/r/programming/comments/5eow80/rusts_standard_hash_table_types_could_go/]
* Incredible amount of different materials on CS topics. Simply treasure. [https://github.com/Developer-Y/cs-video-courses/blob/master/README.md#data-structures-and-algorithms]
* Weird stuff from the author of the one of most popular courses for python newbies. Almost all arguments are like from another planet and tone of the article is so pretentious [https://learnpythonthehardway.org/book/nopython3.html] and great response to it [https://eev.ee/blog/2016/11/23/a-rebuttal-for-python-3/] and sort of post mortem from author [https://zedshaw.com/2016/11/24/the-end-of-coder-influence/]
* Brief introduction to `go` interfaces. [http://www.calhoun.io/how-do-interfaces-work-in-go/]
* Insane overview of distributed systems class from Aphyr. [https://github.com/aphyr/distsys-class]
* Which rules provide for user's passwords in 2016. [https://nakedsecurity.sophos.com/2016/08/18/nists-new-password-rules-what-you-need-to-know/]
* Rapid intro to Lisp. [https://hackernoon.com/learn-you-a-lisp-in-0-minutes-e0c1a060a178#.wnnx02rfe]
* Great reading on very clever simplification for inverse square root and why its important. [http://h14s.p5r.org/2012/09/0x5f3759df.html]
* Yet another stone thrown at python by one of its long term users. I'm kinda agreed with point of view of Jeff in terms that python code is really hard maintainable in bigger scale and in longer terms especially with big teams. Not that its bad language by any means, it just compromises some safety, speed and robustness in favor to simplicity (at least on the surface) and user friendliness. I'm caught by myself not even once that I'm missing static typing in python badly. [https://jeffknupp.com/blog/2016/11/13/how-python-makes-working-with-data-more-difficult-in-the-long-run/]
* Interesting note on trailing slashes for URIs. Never actually fought about such problem, but this week I faced it and decided to do some investigation. This article seems to be the most reasonable explanation. Note that for REST it will slightly differ. [https://cdivilly.wordpress.com/2014/03/11/why-trailing-slashes-on-uris-are-important/]
* This looks so weird that I can't even say what am I thinking about that. This looks interesting and pretentious at the same time. New programming language, new platform, new paradigm? [https://hackernoon.com/how-eve-unifies-your-entire-programming-stack-900ca80c58a7#.hwfuncwxs]
* Semaphore and deadlock explained as easy as it can gets. [https://www.reddit.com/r/compsci/comments/5e08s5/eli5_semaphores_and_the_producerconsumer_problem/]
* Couple of articles on infamous CAP theorem.
 * Briefly what is it all about - [https://jvns.ca/blog/2016/11/19/a-critique-of-the-cap-theorem/] which lead me to next two.
 * Almost the same but with much more details, great reading actually. [https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed].
 * Also great read with similar arguments but with better explanation of terminology and issues and with tons of links to other great materials. [https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html]
* Interesting point of view on how abstractions affect our code with time and different people and why its better to simplify them right from the start and even rewrite with time. [http://www.sandimetz.com/blog/2016/1/20/the-wrong-abstraction]
* Interesting site which promises to provide developer mock interview same as it in those dream companies like Amazon, Uber, Facebook etc. I found its interesting because of blog and couple of articles about pretty cool interview question, generally programming ones. [http://www.gainlo.co/]

#### Articles on other interesting topics
* Valid rant on new USB-C ports and their wide capabilities and why it will be very misleading for regular customers and bad for market in general. [https://geektimes.ru/post/282824/] (__RUS__)
* Got my jaw dropped after reading that something like that exists - prime number which in binary form represents program to brake DVD protection. [https://en.wikipedia.org/wiki/Illegal_prime]
* Flaws of deterministic password managers. And I actually didn't even know that such exists. Anyway after reading this article I don't think that I would try some. [https://tonyarcieri.com/4-fatal-flaws-in-deterministic-password-managers]
* Very interesting reading on ternary machines. I've already read ones about such unique computers and that they were more efficient calculation wise than binary ones. But I never thought actually why. And the most interesting thing for me appeared that worldwide usage of binary systems is explained by easiness and low price of production. Is it possible for all of us to switch to some other systems if we'll actually learn how to make production of those ternary elements cheap? [https://dev.to/buntine/the-balanced-ternary-machines-of-soviet-russia]
* Why Apple's new macbook pros have only 16Gb of RAM? [https://macdaddy.io/macbook-pro-limited-16gb-ram/]
* Interesting confession of mature developer about stuff he did as a youngster. Definitely worth reading and thinking about what ACTUALLY we are doing while write code. [https://medium.freecodecamp.com/the-code-im-still-ashamed-of-e4c021dff55e#.dht7gjk43]
* Great application for Mac OS on recreating pictures in primitive shapes. It looks really awesome! [https://primitive.lol]
