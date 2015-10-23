---
layout: post
title: N-gram models in Scala
comments: true
readmore: true
---

So simple it hurts.  
Thanks to Scala and with the power of functional programming.  

Let's start!  

## Wait! N-gram?

I'll assume you know what a n-gram is. If not, <a href="https://en.wikipedia.org/wiki/N-gram" target="_blank">read this</a>.  
By the way, in **NLP**, a n-gram is what we call a <a href="https://en.wikipedia.org/wiki/Language_model#n-gram_models" target="_blank">Language Model</a> (LM).

## Let's build it, step by step

So, we need some text data to learn from.  
We'll go with a simple example:  

> "The blue sky is near the red koala near the blue sky"

Now, let's tokenize it.  
Here, we will simply split it by using the whitespace:  

<script src="https://gist.github.com/Cowa/005a3b44eb8450bc0315.js"></script>

Then we need to create all the tuples of tokens. In a bigram (2-gram), we need every adjacent couple. For trigram (3-gram), every adjacent triple, etc.  

Well, we can do it in one line with `sliding`: 

<script src="https://gist.github.com/Cowa/e75f0f3a4da4c4ccdb21.js"></script>

We will stick with the bigram for simplicity.

<script src="https://gist.github.com/Cowa/459b4471a7f192b1b41b.js"></script>

For each tuples, we want the probability. To compute it, we first need to count occurences of each unique tuples.  


In Scala, we can simply group elements of the list with `groupBy(identity)` function, which returns a `Map`. And because same tuples have the same identity, they will be grouped together! Then, we have to `mapValues(_.size)` it to get the count.  

Literally one simple line: 

<script src="https://gist.github.com/Cowa/12661077ab116d446e31.js"></script>

And now, we can compute probabilities. It's a little harder, but not so much:  

<script src="https://gist.github.com/Cowa/4b47b3b0e6d46a6cfa73.js"></script>

Nice, right?  

**But** the `ngramWithCount.filterKeys(...).values.sum` is very costy if you call it for each entry of the n-gram... 
When working on medium+ size documents, it will take forever :(

Of course we can fix it, by creating an index for sums: 

<script src="https://gist.github.com/Cowa/dfea4398eda88419cc70.js"></script>

Way faster!

## Make it LM-ready

Now we can predict probabilities of appearance of a word based on the `n` previous word(s). But in a Language Model (LM), it's important to take into consideration the beginning and ending of a sentence.

By convention, we use `<s>` and `</s>` markers. We need to prepend our list of tokens with `n - 1 <s>` and append `1 </s>`.

And that's all! Then we can `sliding(n)` it and use the same functions as before to build the n-gram.  

<script src="https://gist.github.com/Cowa/774b58cbe1e4ebc0abaf.js"></script>

## Turn it into a module

We have all the necessary to turn all of this to a small Scala module to create n-gram models with one line.

<script src="https://gist.github.com/Cowa/9432d825cbfcee57d708.js"></script>

There is nothing new. With this module, the end-user has just one line to write. For example, to create a trigram: 

`val trigram = NGram.build(myTextDataSource, 3)`

And we are done!

## To be continued...

In a next post, we will see how to generate text from these n-gram models. It's really funny and results are even impressive sometimes! Of course, we will need a **BIGGER** source of text to train our n-gram...
