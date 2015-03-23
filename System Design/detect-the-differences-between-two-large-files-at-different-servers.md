[Metadata]
title: Detect the differences between two large files at different servers
date: 2015-03-16 21:47:08 

[Tags]
difficulty: 2
categories: hash, merkle tree
source: hulu

[Description]

Now you have two large file at two different servers. This two files are differ in some data blocks. Please find out a way to detect the different blocks in this two large files.

[Solution]

First of all, let's simplify the problem.

The problem might be easy that how to find out if two files are same of not. MD5 checksum (or other checksum algorithms) will really do the tricks.

However, out problem is a little bit harder because we have to find out the exact different part. And the checksum can only give you the answer of "same or not".

Okay, let's simplify it again. What if the files are differ in one single block?

Think about the binary search, each time you deal with one single piece from two, and detect whether the element you want if in this piece or not. It's quite similar to this scenario. We can calculate the checksums of the same half of the two files. If the checksums is equal, we assume that this part of the file are the same. If not, we turn to detect the next part.

It's the answer for multiple different blocks, either. And it's an effective one because we can usually reuse the checksum of some large data blocks if they are not modified. But this method need network communication every time when we want to check the equality of a single file block. Sometimes the network communication is expensive or even not acceptable if the it is hot and busy.

The Merkle Tree (a.k.a Hash Tree) is a datastructure which designed for this kind of problems. 

We divide the large file into several parts as the leaf of the Merkle Tree and calculate the checksum of them. MD5, SHA-1 are both OK, and if the hash only needs to protest against unintensional damage, CRCs and be used either.

The hash value of parent node in Merkle Tree is the concatenation of the leaf nodes. 

![Merkle Tree Example][1]

There are multiple advantages for using the Merkle Tree:

1. Few network communication is needed. Less time is wasted on the IO wait of the network. And could reduce strain on the network.
2. The hash value of the nodes could be reused if the block is not changed.
3. It's easy to find out the very block which is different from each other. The time complexity is O(logN).

[1]: http://wizmann-pic.qiniudn.com/2b69974b21750e158c6fbceeb4206383
