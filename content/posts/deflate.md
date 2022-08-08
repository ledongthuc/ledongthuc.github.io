---
title: "Deflate algorithm"
series: "software development"
categories: "notes"
summary: "Deflate is a lossless streaming compression/decompression algorithm. It's used widely in GZIP and PNG. Deflate is a combination of LZ77 and Huffman coding algorithm to compress data."
date: 2022-08-08T21:00:00+01:00
draft: false
tags:
- Deflate
- compression
- png
- gzip
---

# Deflate

Deflate is a lossless streaming compression/decompression algorithm. It's used widely in GZIP and PNG.

Deflate is a combination of LZ77 and [Huffman coding](https://thuc.space/posts/huffman_coding_algorithm/) algorithm to compress data.

LZ77 provides a technique for searching duplicated series of bytes in original data and replacing them with back-reference to the first duplicated part position.

[Huffman coding](https://thuc.space/posts/huffman_coding_algorithm/) provides a mapping solution for bit reduction by replacing original data with smaller size code.

## Deflate stream format

Deflate is designed as a series of blocks that you can compress/decompress as a stream sequence.

```
┌─────────────┐
│             │
│    block    │
│             │
├─────────────┤
│             │
│    block    │
│             │
├─────────────┤
│             │
│    block    │
│             │
├─────────────┤
│             │
│    block    │
│             │
├─────────────┤
│             │
│    block    │
│             │
├─────────────┤
│             │
│    block    │
│             │
└─────────────┘
```

Each block has 2 main parts: header and data.

### Block's header

Deflate's block uses 3 bits header as a starting of the block.

```
┌─────────────┐                                         
│     ...     │                                         
│    block    │                                         
│             │                                         
├─────────────┼────────┬───────────────────────────────┐
│             │        │Bit (0) - Last-block-in-stream │
│             │        ├───────────────────────────────┤
│             │ Header │                               │
│             │        │  Bit (1) & (2) - Block type   │
│             │        │                               │
│             ├────────┴───────────────────────────────┤
│             │                                        │
│             │                                        │
│             │                                        │
│    block    │                                        │
│             │                                        │
│             │                                        │
│             │                  Data                  │
│             │                                        │
│             │                                        │
│             │                                        │
│             │                                        │
│             │                                        │
│             │                                        │
├─────────────┼────────────────────────────────────────┘
│     ...     │                                         
│    block    │                                         
│             │                                         
└─────────────┘                                         
```

The first bit (0): define the current block

| Value of bit (0) | Description |
|--------|------|
| 0 | This is the last block of the stream. |
| 1 | There are more blocks after this block. |

The second bit and the third bit (1) (2): define block type

| Value of bit (0) | Description |
|--------|------|
| 00 | No compression. Raw data is stored with lengths from 0 to 65535 bytes. It's used for incompressible data. |
| 01 | Compressed with fixed Huffman codes. It used a pre-defined/pre-agreed Huffman tree defined in the Deflate RFC. This type is used for short messages, where a pre-defined Huffman tree saved more space instead a custom Huffman tree. |
| 10 | Compressed with dynamic Huffman codes. Huffman tree will be defined included. |
| 11 | This file is reserved, and don't use. |

### Data

There are two types of data strucutres depending on the header definition, no compression data (00) and compression data (01 and 10).

#### No compression data block

No compression with the block type.

It consists of three parts: length, one's complement of length and literal data.

```
┌─────────────┐                                         
│     ...     │                                         
│    block    │                                         
│             │                                         
├─────────────┼────────┬───────────────────────────────┐
│             │        │Bit (0) - Last-block-in-stream │
│             │        ├───────────────────────────────┤
│             │ Header │                               │
│             │        │      Bit (1) & (2) = 00       │
│             │        │                               │
│             ├────────┼───────────────────────────────┤
│             │        │        2 bits - Length        │
│             │        │                               │
│Non-compresse│        ├───────────────────────────────┤
│  d blocks   │        │ 2 bits - one's complement of  │
│             │        │            length             │
│             │        ├───────────────────────────────┤
│             │  Data  │                               │
│             │        │                               │
│             │        │                               │
│             │        │         literal data          │
│             │        │                               │
│             │        │                               │
│             │        │                               │
├─────────────┼────────┴───────────────────────────────┘
│     ...     │                                         
│    block    │                                         
│             │                                         
└─────────────┘                                         
```

#### Compression data block

Compression data block requires 2 things: Huffman code trees and encoded data.
 - Huffman code trees: use to map original data to reduced size codes. With header definition. Deflate support 2 cases to define the Huffman code tree:
     - Fixed Huffman codes: pre-agreed Huffman code trees from spec.
     - Dynamic Huffman codes: dynamic Huffman code trees that build from raw data.
 - Compressed data: They are a series of elements of 2 types: literal bytes and pointer to back duplicate literal bytes. The compressed data are encoded to a reduced bit of Huffman code trees.

```
┌─────────────┐                                         
│     ...     │                                         
│    block    │                                         
│             │                                         
├─────────────┼────────┬───────────────────────────────┐
│             │        │Bit (0) - Last-block-in-stream │
│             │        ├───────────────────────────────┤
│             │ Header │                               │
│             │        │      Bit (1) & (2) = 01       │
│             │        │                               │
│             ├────────┼───────────────────────────────┤
│             │        │        2 bits - Length        │
│ compressed  │        │                               │
│  blocks -   │        ├───────────────────────────────┤
│Fixed Huffman│        │ 2 bits - one's complement of  │
│    codes    │        │            length             │
│             │        ├───────────────────────────────┤
│             │  Data  │                               │
│             │        │                               │
│             │        │                               │
│             │        │         Encoded data          │
│             │        │                               │
│             │        │                               │
│             │        │                               │
├─────────────┼────────┼───────────────────────────────┤
│             │        │Bit (0) - Last-block-in-stream │
│             │        ├───────────────────────────────┤
│             │ Header │                               │
│             │        │      Bit (1) & (2) = 01       │
│             │        │                               │
│             ├────────┼───────────────────────────────┤
│             │        │        2 bits - Length        │
│             │        │                               │
│             │        ├───────────────────────────────┤
│ compressed  │        │ 2 bits - one's complement of  │
│  blocks -   │        │            length             │
│   Dynamic   │        ├───────────────────────────────┤
│Huffman codes│        │                               │
│             │        │      Huffman code trees       │
│             │  Data  │                               │
│             │        ├───────────────────────────────┤
│             │        │                               │
│             │        │                               │
│             │        │                               │
│             │        │         Encoded data          │
│             │        │                               │
│             │        │                               │
│             │        │                               │
├─────────────┼────────┴───────────────────────────────┘
│     ...     │                                         
│    block    │                                         
│             │                                         
└─────────────┘                                         
```

## Compression stages:

Deflate compress the data in two stages: duplicate elimination and bit reduction.

### Duplicate elimination

In the first stage, deflate uses the LZ77 algorithm. It searches duplicated uncompressed bytes and replaces them with reference pointer. The reference pointer is defined by 2 elements:
 - Distance (or offset): the relative back to the first existing bytes occurs in original uncompressed data.
 - Length: the duplicated bytes length.

The reference pointers of LZ77 in Deflate defines by 8-bit for duplicated uncompressed length and a 15-bit distance of duplicated uncompressed position.

LZ77 can search and create reference pointers for duplicated bytes across blocks, as long as the distance appears within the last 32 KB of uncompressed data decoded.

Let's try an example with original data:

```
This is


           Length: 2 
           ───       
┌─┬─┬─┬─┬─┬─┬─┐      
│t│h│i│s│ │i│s│      
└─┴─┴─┴─┴─┴─┴─┘      
     ▲     │         
     │     │         
     └─────┘         
     Distance: back 3
```

We will have a reference pointer with length = 2 and distance = 3

```
This (3,2)
```

or

```
This (back 3, length 2)
```

## Bit reduction

Deflate uses the Huffman coding algorithm to reduce the size of the original data to smaller bits. If you want to know how Huffman coding works, check https://thuc.space/posts/huffman_coding_algorithm/.

For each block, Deflate define 2 Huffman code trees for mapping code to literal data:
 - The first tree for mapping alphabet and duplicated length of pointer LZ77.
   - Code from 0 to 255: the alphabet.
   - Code 256: indicates end-of-block.
   - Code from 257 to 285: represent the length of pointer reference from 3 to 258 (with extra bits support). It's also the maximum back reference of pointer 258 bytes.
 - Second tree for the back distance of pointer LZ77.
   - Code from 0 to 29: represent back distance of pointer reference from 1 to 32768 (with extra bits support). 

Extra bits support bits that follow encoded code, which allows code can contain more possible values from one code.

 - Example length of pointer reference extra bits:

| Code (decimal) | Code without extra bits | Code with extra bits | Distance |
|--------|------|------|------|
| 265 | 100001001 | 100001001**0** | 11 |
| 265 | 100001001 | 100001001**1** | 12 |
| 274 | 100010010 | 100010010**000** | 43 |
| 274 | 100010010 | 100010010**001** | 44 |

- Example distance of pointer reference extra bits:

| Code (decimal) | Code without extra bits | Code with extra bits | distance |
|--------|------|------|------|
| 6 | 110 | 110**00** | 9 |
| 6 | 110 | 110**01** | 10 |
| 6 | 110 | 110**10** | 11 |
| 6 | 110 | 110**11** | 12 |

Finally, data will be encoded to reduced-size code, including the huffman coding tree if it's explicited in dynamic huffman codes compression data block.

# References:
 - https://en.wikipedia.org/wiki/LZ77_and_LZ78
 - https://en.wikipedia.org/wiki/Huffman_coding
 - https://jvns.ca/blog/2015/02/22/how-gzip-uses-huffman-coding/
 - https://datatracker.ietf.org/doc/html/rfc1951
 - http://www.infinitepartitions.com/art001.html
 - https://zlib.net/feldspar.html
 - https://www.w3.org/Graphics/PNG/RFC-1951
 - https://www.programiz.com/dsa/huffman-coding
