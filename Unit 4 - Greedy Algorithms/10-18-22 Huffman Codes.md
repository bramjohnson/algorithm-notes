# Data Compression
How do we store strings of text compactly?
- Have a binary mapping from alphabet to binary (fixed size)

Letters have *uneven* frequencies. Therefore, we should use less bits for infrequent characters and more bits for frequent characters.

We want <= 4 bits per character, as well as easy encoding and decoding.

**Prefix Free Codes**: characters do not need to start with some prefix.
We can represent it using a binary tree, where each child node is a character and has no children.

# Huffman Codes
Algorithm to find an optimal prefix-free code. Optimality is defined by $min(len(f_i * len_T(i)))$. 

## Attempt 1
**IDEA**: balanced binary tree should have low depth = small lengths
**Greedy Rule**: split letters into two sets of roughly equal frequency and repeat (greedy rule).
- This rule doesn't always work, because one side may have a huge number and a really small number, which would not be efficient if we have a lot of other larger frequency characters on the other side.

## Attempt 2
**Greedy Rule**: Pair up two letters with lowest frequency and repeat
- When two characters are paired, they become one with their frequency summed.