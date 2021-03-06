---
layout: post
title: Message Passing in Elixir, Clojure and Go
---

I have spent some time recently learning Elixir. I have always been interested in Erlang, but the syntax has put me off in the past. One of the examples in the book Programming Elixir involves creating 1,000,000 processes and sending and incrementing an integer through the chain. The Elixir code is:

``` elixir
# Excerpted from "Programming Elixir",
# published by The Pragmatic Bookshelf.
defmodule Chain do
  def counter(next_pid) do    
    receive do
      n ->
        send next_pid, n + 1
    end
  end

  def create_processes(n) do
    last = Enum.reduce 1..n, self,
             fn (_,send_to) ->
               spawn(Chain, :counter, [send_to])
             end

    # start the count by sending
    send last, 0

    # and wait for the result to come back to us
    receive do
      final_answer when is_integer(final_answer) ->
        "Result is #{inspect(final_answer)}"
    end
  end

  def run(n) do
    IO.puts inspect :timer.tc(Chain, :create_processes, [n])
  end
end
```

This run in about 8-9sec on my Macbook Pro for 1,000,000 processes.

I was interested in how this would work in Clojure using core.async. I came up with the following code that I think closely follows the intent of the Elixir code.

``` clojure
(ns scratch.core
(:require [clojure.core.async :as async :refer [go <! chan >!! <!! >!]])
(:gen-class))

(defn inc-and-pass-on [in out]
  (go (let [n (<! in)]
      (>! out (inc n)))))

(defn run-it [n]
  (let [chans (partition 2 1 (repeatedly n chan))
        start (first (first chans))
        end (last (last chans))]
    (doseq [c chans]
      (let [[in out] c]
        (inc-and-pass-on in out)))
    (>!! start 1)
    (println "result is " (<!! end))))

(defn run []
    (let [start (. System currentTimeMillis)
          _     (run-it 1000000)
          end   (. System currentTimeMillis)]
    (println "Ran in" (- end start) "ms")))

(defn -main [& args]
  (run))
```

I was very impressed that this took about the same time as the Elixir code, after a few runs to allow the Hotspot optimisations to kick in. The non-blocking nature of Clojure's core.async go blocks are impressive. This also seemed to take about the same amount of memory as the Elixir version.

Lastly I thought I would have a try at doing this in Go, as core.async follows a similar CSP model as Go. I'm not very familar with Go, but quickly came up with the following:

``` go
package main

import "fmt"
import "time"

func inc(in chan int, out chan int) {
  var i int
  i = <- in
  out <- i + 1
}

func main() {
  start := time.Now()
  const n = 1000000
  var chans [n]chan int
  for i := 0; i < n; i++ {
    chans[i] = make(chan int)
  }

  for i:= 1; i < n; i++ {
    go inc(chans[i-1], chans[i])
  }

  chans[0] <- 1
  result := <- chans[n-1]
  end := time.Now()
  fmt.Println("Result is ", result, " and it took ", end.Sub(start))
}
```

Using Go 1.4 the compiled binary executes this in about 2.3sec. I am very surprised with this, how fast Go is compared with the others. It seems to use about the same memory too as the Elixir version. About 2 Gig. If only I prefer the more functional style of Elixir/Clojure. Go still allows shared mutable state issues without care, though worth investigating a bit more given these results.

Disclaimer: Take micro benchmarks with a grain of salt.
