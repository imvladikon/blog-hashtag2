---
layout: post
title:  "First post, generate all permutations of string"
date:   2019-10-15 09:45:16 +0200
categories: 
description: 
tags: java algorithms
---
Hi, everyone! I've started my own blog;)

<img src="/assets/img/good_news_everyone.jpg" alt="good_news_everyone" width="250"/>

It's first post about banal old exercise, given a string, generate all permutations of it
Here's solution on java
```java
public static Stream<List<String>> permute(List<String> input) {
        if (input.size() == 1) {
            return Stream.of(input);
        } else {
            return input.stream().flatMap(el ->
                    permute(input.stream().filter(j -> j != el).collect(toList()))
                            .peek(l -> l.add(0, el)));
        }
    }
```
<!--break-->

