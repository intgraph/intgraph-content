[Metadata]
title: 6 Pirates
date: 2017-08-19 15:43:13

[Tags]
difficulty: 3.5
categories: mathmatical induction
source: The Algorithm Design Manual

[Description]

6 pirates must divide 300 gold coins among themselves. The division is to proceed as follows:

1. Mark the pirates as No.1 to No.6
2. The pirate who has the maximal number process the way to divide the coins
3. Then all pirates vote.
4. If the pirate get at least half votes, he survives and the division remains.
5. Otherwise, he is killed.
6. The pirates repeats step 2 to step 4 until a division is accepted.

If all pirates are intelligent and the first priority is to stay alive and get as much as money as possible, what will happen during that process?

> Source: The Algorithm Design Manual, Cpt2, Exercise 2-51

[Solution]

The problem is twisted at the very first attempt if you start with 6 pirates situation. Because the rest 5 pirates who vote can either accept the division or sentence the No.1 pirates to death to get more coins.

But we can start from a simplified situation. What if there're only 2 pirates?

It's quite easy to understand that the No.1 pirate will get all while the No.2 pirate will get nothing. Because No.1 pirate will definitely win the vote which of course is in the best interest for himself.

And for 3 pirates situation, the No.3 pirate who has the smallest number knows that if No.1 pirate is killed, he will get nothing. So if No.1 what survive, he should satisfy No.3 with ... ONE coin, at least it's better than nothing, right? As a result, at that vote, No.1 pirate get 299 coins and No.3 pirates get 1 coin, and both of them will fully support that division.

So we can get the answer step by step as we know that a pirate will support a division if he can get more.

![](http://wizmann-pic.qiniudn.com/17-8-19/78418745.jpg)

From the that table, we can make a conclusion that the pirate can make at most `300 - (n - 1) / 2` coins, and here is the way we can prove it.

1. Induction hypothesis:    
> For a situation of `n` pirates, there will be `(n - 1) / 2` pirates get **one single coin**, and pirate with the largest number will get all the rest. And the division will be passed because there will be at least half pirates vote for it.

2. From the table below, we know for the initial several cases the hypothesis is true.

3. Suppose the situation of `n` pirates is true, there is `(n + 1) / 2` pirates get no money at all. So in the `n + 1` pirates situation, firstly we will give the `(n + 1) / 2` pirates one single coin to make them vote for the division, and the pirate with the largest number, the pirate `n + 1`, get all the rest of the coins. And it's easy to prove that `(n + 1) / 2 + 1 >= n / 2`.
4. Our hypothesis is well proved.