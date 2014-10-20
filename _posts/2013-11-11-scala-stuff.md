---
layout: post
title: The Future is Scala?
---

I've been really busy recently hence the lack of posting. I have been doing the Coursera Scala programming course. Along side a few other Scala related things. I have also been reading through the great book Scala for the Impatient, I highly recommend this book as the best introduction to Scala out there at the moment.

Scala really interests me, though it has more to do with the fact that I really want to believe that there is a better way to construct software that doesn't involve threads and mutable state. The combination of threads and mutable state, has been killing me on the product that I work on in my day job. This Java application was written in the late 90's and early 2000's, when the solution to everything seemed to be add more classes and threads. Oh, it didn't work, add more syncronized decorators to methods! This has lead over the years to code that is incredibly hard to debug, test and reason about how it will perform in production.

Enter Scala, it promises to solve all these problems and still run on the JVM and be able to leverage our existing code base.

So far I have been impressed, though the size and complexity of the language reminds me of the early days of C++. I think to deploy Scala into a large enterprise team, you would need some strict coding guidelines and pick the parts of the language that the team will stick with.

Thought some of the presentations that were given at the ScalaDays conference this year were worth watching.

http://www.parleys.com/channel/51ae1022e4b01033a7e4b6ca/presentations?sort=date&state=public

The three ones that I found interesting were:
1. Going Reactive: Scalable, Highly Concurrent & Fault­Tolerant Systems by Jonas Bonér 
2. Concurrency – The good, the bad, the ugly by Viktor Klang & Roland Kuhn
3. Keynote ­ Scala in 2018 by Rod Johnson

I thought that 1 & 2 were very interesting, talking about the Scala/Akka approach to concurrency and scalability.

One that impressed me greatly was Rod Johnson's keynote. I really believe that for Scala to crack the popularity contest, it needs to focus on getting more adoption before Java 8 with its Lambda support arrives on the scene. I would like to see the following improved in Scala:
* Market it as a better, more concise Java to start with. Like C++ was used coming from C in the early days. Don't start with trying to be a Haskell running on the JVM, like some supporters of the language go with to start. Once people start using it, then introduce more functional style of doing things.
* SBT is terrible. Clojure's lein tool is much nicer. This needs to be fixed for wider Scala adoption.
* The Scala docs are rather hard to navigate. You never know if what you are looking for is in the class, object, package object, some implicit class, etc... Can take a while to track done a function you are after.

That being said, I really believe something like Scala and its Actor support is the way forward.

I have been working through the Play book and am really impressed so far with the framework. Though I think it needs a better database access story.
Once I am finished with the Play book, I'm going to go through Web Development With Clojure. As Clojure really interests me too, with its slightly different take on concurency.
