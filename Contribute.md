# How to contribute

IntGraph is a cooperative project to share the interview problems. So if you have a good problem, no matter you read it from a book or you hear it on the street or get it at some other places, feel free to add it to the list. I'll be grateful.

## How to add a problem

The content of the website is host on [Github][1]. You can send a pull request to update the problem list or fix some typos or mistakes.

## The format of the problem

I create a new type of markdown by pyparsing, and I consider that the "new language" is not bad. :)

There are four sessions of this file.

* Metadata

Indicate the title and the adding time of this problem.

* Tags

The tags are used to filter the problem from the whole list. And there are three kind of tags. First one is "difficulty". And the second is "categories", which indicates the type of the problem. The last one is "source", which tells the origin of the problem. (It's OK to have a "unknown" source if you don't quite clear where this problem is from.)

* Description

The description of the problem, and I know it's easy to understand. :)

* Solution

The solution of the problem, and it will be hide before the readers click the "show" button. It's the right place to put the answers.

## The difficulty of the problem

* Difficulty - 1

This level is for the problems with **no algorithm**, and **easy to write**. Such as "a + b problem" / "find the max number of an array". Usually, no interview problem is in this level.

* Difficulty - 2

This level is for the problem with some simple algorithm or some problems which is a little bit hard to write.

For example, the "[Add Two Numbers][2]" form Leetcode, and the "[Jzzhu and Children][3]" from Codeforces.


* Difficulty - 3

Some problem with common difficulty. You have to come up with a good algorithm or write the code carefully to deal with it. This is the level for the **not too hard** problem.

For example, the "[Jzzhu and Chocolate][4]" from Codeforces.

* Difficulty - 4

You have to come up with one of the best idea to tackle the problem and write some good code to satisfy the interviewer. If you can deal with the problem compeletely, you may get a good offer for some of the big companies.

This level is for some hard algorithm problem and some of the system design problems which need your talent to deal with them.

For example, the "[Little Pony and Harmony Chest][5]" from Codeforces or "Design a search engine for Linkedin".

* Difficulty - 5

You are the MASTER!

## Example

    [Metadata]
    title: A + B problem
    date: 2014-08-10 23:13:31 
    
    [Tags]
    difficulty: 1
    categories: simulation, no algorithm
    source: unknown
    
    [Description]
    This is a example problem. Just calculate a + b.
    
    [Solution]
    The solution here is ready to play.
    ```python
    import sys
    for line in sys.stdin:
        (a, b) = map(int, line.split())
        print a + b
    ```



[1]: https://github.com/intgraph/intgraph-content
[2]: https://oj.leetcode.com/problems/add-two-numbers/
[3]: http://codeforces.com/contest/450/problem/A
[4]: http://codeforces.com/contest/450/problem/C
[5]: http://codeforces.com/problemset/problem/453/B
