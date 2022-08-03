---
title: "Huffman coding algorithm"
series: "software development"
categories: "notes"
summary: "The Huffman coding is an algorithm that is used for lossless data compression, such as gzip or PNG image format."
date: 2022-07-05T21:00:00+01:00
draft: false
socialImage: "https://thuc.space/images/huffman_coding_algorithm/huffman_coding_algorithm.png"
tags:
- Huffman coding
- compression
- png
- gzip
---

The Huffman coding is an algorithm that is used for lossless data compression, such as gzip or PNG image format.

The Huffman coding replaces symbols from original document to mapping codes. The mapping codes' can be different sizes, from 1 bit until unlimited bits.

The mapping codes' size is smaller than the original symbol's size. This technique provides the Huffman coding algorithm ability to compress data, from bigger-size original symbols to smaller-size codes.

Example original codes' sizes and mapping sizes:

| Symbol | Code |
|--------|------|
| a (8 bits) | 0 (1 bit) |
| b (8 bits) | 10 (2 bit) |
| c (8 bits) | 11 (2 bit) |
| d (8 bits) | 110 (3 bit) |
| e (8 bits) | 111 (3 bit) |

The mapping codes can have different sizes. The more common-occurred symbols are prioritized to use smaller code (fewer bits) than less common symbols. It helps more common-occurred symbols are reduced by more bytes by their codes.

| Symbol |  Total of occurs | Code |
|-|-|-|
| a (8 bits) | 10 times (80 bits) | 0 (1 bit x 10 = 10 bits) |
| b (8 bits) | 7 times (56 bits) | 10 (2 bit x 7 = 14 bits) |
| c (8 bits) | 5 times (40 bits) | 11 (2 bit x 5 = 10 bits) |
| d (8 bits) | 2 times (16 bits) | 110 (3 bit x 2 = 6 bits) |
| e (8 bits) | 1 times (8 bits) | 111 (3 bit x 1 = 3 bits) |

The codes in the huffman coding algorithm is prifix code. It means no any code is a prefix of another code. It avoid to confusing between encode/decode symbols and series of codes. You can check the table of "original codes' sizes and mapping sizes", no mapping codes is a prefix of other codes.

## Basic steps to compress data with the Huffman coding algorithm

1. Analyse symbols and their weight (number of occurs) from an original document.
2. Build a binary tree. Middle nodes contain temporary code, leaves contain mapping between symbols and their mapping codes. Keeps highest weight symbol on one side of the binary tree, lower weight symbol try to add to another side of the binary tree.
3. Collect leaves and their code. Replace symbols with codes.

## Example

Original document:

```
xyzxnxyzxxmyxzxyzxxyzxyy
```

Bits view:

```
011110000111100101111010011110000110111001111000011110010111101001111000011110000110110101111001011110000111101001111000011110010111101001111000011110000111100101111010011110000111100101111001
```

1. Symbols and weights:

| Symbol | Weight (number of occurs) |
|-|-|
| x | 10 |
| y | 7 |
| z | 5 |
| m | 2 |
| n | 1 |

2. Build tree:

```
                  ┌──────┐                                              
                  │ Root │                                              
                  └──────┘                                              
                      │                                                 
                      ├──────1─────┐                                    
                      │            ▼                                    
                      0      ┌──────────┐                               
                      │      │   Temp   │                               
                      │      │ code: 1  │                               
                      │      │          │                               
                      │      └──────────┘                               
                      │            │                                    
                      │            ├─────1─────┐                        
                      │            │           ▼                        
                      │            0     ┌──────────┐                   
                      │            │     │   Temp   │                   
                      │            │     │ code: 11 │                   
      ┌───────────────┘            │     │          │                   
      │                            │     └──────────┘                   
      │                            │           │                        
      │                            │           ├─────1────┐             
      │              ┌─────────────┘           │          ▼             
      │              │                         0    ┌──────────┐        
      │              │                         │    │   Temp   │        
      │              │                         │    │code: 111 │        
      │              │              ┌──────────┘    │          │        
      │              │              │               └──────────┘        
      │              │              │                     │             
      │              │              │                  0  │   1         
      │              │              │              ┌──────┴───────┐     
      │              │              │              │              │     
      ▼              ▼              ▼              ▼              ▼     
┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
│symbol: x │   │symbol: y │   │symbol: z │   │symbol: m │   │symbol: n │
│weight: 10│   │weight: 7 │   │weight: 5 │   │weight: 2 │   │weight: 1 │
│code: 0   │   │code: 10  │   │code: 110 │   │code: 1110│   │code: 1111│
└──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘
```

3. Create code mapping

| Symbol | Code |
|-|-|
| x | 0 |
| y | 10 |
| z | 11 |
| m | 110 |
| n | 111 |

Replaced document (bits view):

```
01011001111010110001110100110010110001011001010
```
