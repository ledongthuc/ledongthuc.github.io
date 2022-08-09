---
title: "Gzip"
series: "software development"
categories: "notes"
summary: "GZIP is a lossless compression and decompression method. it supports the compression and decompression of a data stream to produce another data stream. It is based on Deflate that combines of LZ77 and Huffman coding"
date: 2022-08-09T20:00:00+01:00
draft: false
tags:
- gzip
- compression
- png
- gzip
---

GZIP is a lossless compression and decompression method.

GZIP supports the compression and decompression of a data stream to produce another data stream.

GZIP is based on [DEFLATE](/posts/deflate/) algorithm that is a combination of LZ77 and [Huffman coding](/posts/huffman_coding_algorithm/).

# 1. GZIP format:

Gzip format structure includes
 - 4 main members: header, compressed blocks, compliance.
 - 4 optional members: Extra fields, file name, file comment and CRC16. They are controlled by flags in the header section.

```
┌──────────────┐
│              │
│              │
│   Header &   │
│   Trailer    │
│              │
│              │
├──────────────┤
│              │
│ Extra fields │
│  (optional)  │
│              │
├──────────────┤
│              │
│  File name   │
│  (optional)  │
│              │
├──────────────┤
│              │
│ File comment │
│  (optional)  │
│              │
├──────────────┤
│              │
│    CRC16     │
│  (optional)  │
│              │
├──────────────┤
│              │
│              │
│              │
│              │
│  Compressed  │
│    blocks    │
│              │
│              │
│              │
│              │
│              │
├──────────────┤
│              │
│              │
│    CRC32     │
│              │
│              │
│              │
├──────────────┤
│              │
│              │
│    ISIZE     │
│              │
│              │
│              │
└──────────────┘
```

## 1.1. Header & trailer

```
┌──────────┬─────────────────────┐
│          │     ID1 (0x1f)      │
│          ├─────────────────────┤
│          │     ID2 (0x8b)      │
│          ├─────────────────────┤
│          │ Compression method  │
│          ├─────────────────────┤
│          │        Flags        │
│          ├─────────────────────┤
│          │                     │
│          │                     │
│ Header & │                     │
│ Trailer  │                     │
│          │  Modification time  │
│          │                     │
│          │                     │
│          │                     │
│          │                     │
│          │                     │
│          ├─────────────────────┤
│          │     eXtra flags     │
│          ├─────────────────────┤
│          │  Operating system   │
├──────────┼─────────────────────┘
│          │                      
│   ...    │                      
│          │                      
└──────────┘                      
```

 - ID1 (1 byte) = magic number: 0x1f. Used to identify the gzip format.
 - ID2 (1 byte) = magic number: 0x8b. Used to identify the gzip format.
 - Compression method (1 byte):
    - 8 = deflate
    - 0 -> 7 = reversed
 - FLG (1 byte): defines 8 bit flags (read next section for detail).
 - Modification time (4 bytes): most recent modification time of the original file being compressed. If compressed data is not from the file, modification time = compression time.
 - eXtra FLags: defines extra compression configuration. Size is flexible and is defined in 2 bits. If the compression method is 8 (deflate), we can set this field with the following values
   - 2 = compressor used maximum compression, slowest algorithm
   - 4 = compressor used the fastest algorithm
 - OS: Define the operation system in which compression took place. It's used for determining end-of-line character. Supported values:
   - 0 = FAT filesystem (MS-DOS, OS/2, NT/Win32)
   - 1 = Amiga
   - 2 = VMS (or OpenVMS)
   - 3 = Unix
   - 4 = VM/CMS
   - 5 = Atari TOS
   - 6 = HPFS filesystem (OS/2, NT)
   - 7 = Macintosh
   - 8 = Z-System
   - 9 = CP/M
   - 10 = TOPS-20
   - 11 = NTFS filesystem (NT)
   - 12 = QDOS
   - 13 = Acorn RISCOS
   - 255 = unknown

### 1.1.1. FlG - Flags

```
┌──────────┬─────────┐                      
│          │         │                      
│          │   ...   │                      
│          │         │                      
│          ├─────────┼─────────────────────┐
│          │         │  Bit 0 - FTEXT      │
│          │         ├─────────────────────┤
│          │         │  Bit 1 - FHCRC      │
│          │         ├─────────────────────┤
│          │         │  Bit 2 - FEXTRA     │
│          │         ├─────────────────────┤
│ Header & │         │  Bit 3 - FNAME      │
│ Trailer  │  Flags  ├─────────────────────┤
│          │         │  Bit 4 - FCOMMENT   │
│          │         ├─────────────────────┤
│          │         │  Bit 5 - reversed   │
│          │         ├─────────────────────┤
│          │         │  Bit 6 - reversed   │
│          │         ├─────────────────────┤
│          │         │  Bit 7 - reversed   │
│          ├─────────┼─────────────────────┘
│          │         │                      
│          │   ...   │                      
│          │         │                      
└──────────┴─────────┘                      
```

 - bit 0, FTEXT = if it's set, the file is probably ASCII text.
 - bit 1, FHCRC = if it's set, CRC16 for the gzip header is present, placed before the compressed data. CRC16 contains 2 bytes of CRC32 for all bytes of gzip header.
 - bit 2, FEXTRA = if it's set, extra fields section are added in next sections.
 - bit 3, FNAME = if it's set, file name section are added in next sections.
 - bit 4, FCOMMENT = if it's set, comment section are added in next sections.
 - bit 5 = reserved, don't use it.
 - bit 6 = reserved, don't use it.
 - bit 7 = reserved, don't use it.

## 1.2. Extra fields

If FLAG.FEXTRA, bit 2 in the header section, is enabled, and an extra fields section is added to the next section.

```
┌──────────┐                      
│          │                      
│   ...    │                      
│          │                      
├──────────┼─────────────────────┐
│          │         SI1         │
│          ├─────────────────────┤
│          │         SI2         │
│          ├─────────────────────┤
│          │                     │
│          │         LEN         │
│          │                     │
│          ├─────────────────────┤
│          │                     │
│          │                     │
│  Extra   │                     │
│  fields  │                     │
│          │                     │
│          │                     │
│          │        data         │
│          │                     │
│          │                     │
│          │                     │
│          │                     │
│          │                     │
│          │                     │
│          │                     │
├──────────┼─────────────────────┘
│          │                      
│   ...    │                      
│          │                      
└──────────┘                      
```

Extra fields' structure has:
- SI1 (1 byte) and SI2 (1 byte): is sub-field IDs, that are defined by 2 ASCII characters.
- LEN (2 bytes): is the size of extra fields sections, included SI1, SI2 and LEN.

## 1.3. File name

If FLAG.FNAME, bit 3 in the header section, is enabled, and a file name section is added to the next section.

The file name section is a series of bytes and it's terminated by a zero byte. It follows ISO 8859-1 (LATIN-1) characters. If the source file is from stdin, there is no file name.

## 1.4. File comment

If FLAG.FCOMMENT, bit 4 in the header section, is enabled, and a file comment section is added to the next section.

The file comment section is a series of bytes and it's terminated by a zero byte. It follows ISO 8859-1 (LATIN-1) characters.

## 1.5. CRC16

If FLAG.FHCRC, bit 1 in the header section, is enabled, CRC16 section is added to the next section.

CRC16 is a Cyclic redundancy check 17 bits, an error-detecting code with 17 bits to detect accidental changes to digital data. In this case, it detects accidental changes in header section.

The CRC16 consists of the two least significant bytes of the CRC32 for all bytes of the gzip header up to and not including the CRC16.

## 1.6. Compressed blocks

The main part of the file is compressed blocks that use [deflate algorithm](/posts/deflate/) (or anything else that's defined in the header).

Deflate is designed as a stream of blocks that support lossless compression/decompression streaming.

Detail of please check reference: https://thuc.space/posts/deflate/

## 1.7. CRC32

CRC32 is a Cyclic redundancy check 33 bit, an error-detecting code with 33 bits to detect accidental changes or wrong bit transmission of original uncompressed data.

## 1.8. ISIZE

This contains the size of the original (uncompressed) input data modulo 2^32

```
ISIZE = uncompressed_size % (2^32)
```


# 2. References:
 - https://en.wikipedia.org/wiki/Gzip
 - https://datatracker.ietf.org/doc/html/rfc1952
 - https://en.wikipedia.org/wiki/Streaming_algorithm
 - https://en.wikipedia.org/wiki/Deflate
 - https://en.wikipedia.org/wiki/Cyclic_redundancy_check
 - http://www.sunshine2k.de/articles/coding/crc/understanding_crc.html#ref1
