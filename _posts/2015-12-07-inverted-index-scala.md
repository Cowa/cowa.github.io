---
layout: post
title: Inverted index in Scala
comments: true
readmore: true
---

Last week I had to build an inverted index to speed up (a lot) a program doing parallel document identification.
And I was quite impressed by how simple it was in Scala!

# Inverted index?

It's simple, let's say you have this index:

<script src="https://gist.github.com/Cowa/d8cfc4f4ea32d5ad6d11.js"></script>

For each line you have a document identifier followed by the words appearing in the document. Here, `word1` is in `document0.txt`, but not in `document1.txt`.

And you want to turn this index into:
<script src="https://gist.github.com/Cowa/3c4a1fb475083ce49bf7.js"></script>

For each word you have the documents in which it appears. Here, `word0` appears in `document0.txt` and `document1.txt`.

This is called an **inverted index**. And it's pretty useful.

# Let it code

Don't waste more time. Here is the code.  
There is literally more comments than code ;)

<script src="https://gist.github.com/Cowa/c3c7fc4d9cb60843ad37.js"></script>

All you have to do is: `InvertedIndex("path-to-my-index.txt")`

I love Scala <3
