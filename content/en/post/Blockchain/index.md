+++
author = "Hugo Authors"
title = "Exploring the Bitcoin's blockchain with Rust - L3"
date = "2022-01-01"
description = "Using Rust to browse the Bitcoin's registers"
tags = [
    "Rust",
    "Blockchain",
    "Bitcoin",
    "Multithreading"
]
categories = [
    "Rust",
    "Bitcoin",
    "Bachelor's degree",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
image = "intro.png"
toc = true

+++
[Lien GitLab](https://gitlab.com/AlyssaShep/blockchain-rust.git)
<!-- [![Lien projet](logo.png)](https://gitlab.com/AlyssaShep/blockchain-rust.git) -->

## Description

This porject is a text interface that calculate and display statistics (number of transactions,...) extracted from the Bitcoin's blockchain with Rust.

## Introduction

This project offer the discovery of two univers that's just emerged : Bitcoin's blockchain and Rust. The topic was to manipulate the registers of this cryptocurrencies using a language we didn´t know a thing about. This project was a collaboration with some Polytech's teachers from Montpellier.

## Features

{{< gallery match="images/*" sortOrder="asc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=false previewType="blur" embedPreview=true loadJQuery=true >}}

This graphic interface is accessible from a UNIX terminal, Windows or even a IDE terminal. The interface is responsive : it adapts when the user change the window dimension. It is divided into 5 tabs :

+ the Overview tab : 
    + presents briefly the software
    + reminds of the commands that can be done to launch the program differenlty
    + displays the key to access the "help me" tab for more intel on how to browse

+ the Block tab :
    + display the general information of a block :
        + its size, number of transactions, ...
        + version, Merkle's root, timestamp, bits, nonce
        + Some statistics (exchange's volume, average and median, the first and thrid quartile)

+ the Transaction tab displays a curve of the number of transactions per block

+ the BTC tab displays a curve of the biggest amount of BTC per block

+ the help me tab explains how to use the application : how to switch tabs, and to use the others features we've got (next registers,...).

We've added a progress bar on the data's export, the display of the terminal and even for the parser with the name of the registers it's working on. For the export of the data, the user can choose between json, txt or cancel the export.

For the curve, we've used a point cloud. This one allows us to see the results of each block at the same time. On the x-axis, we've got the block's number and on the y-axis, the number of blocks. This allowed us to have a greater recoil and impact on all the blocks.

## Démo

{{< video src="demo.mp4" controls="yes" >}}

## How to use

### Crates

We've used the following crates :
+ chrono :  a library for the time's management who allows to interpret Unix timestamp contained inside a block's header.
+ crossterm : a library to handle a terminal
+ hex-string : a struct that convert all vectors of integer into a heximal string. Really useful because the Bitcoin's blockchain use a lot of base 16 for the key's editing (private and public keys, signature and hash)
+ sha2 : implement the hash function of the SHA-2 family. We use the SHA-256 for identifing blocks or transactions
+ tui-rs : a library for building interfaces et boards inside a terminal, it's divided and handled with some widgets.

### Installations

+ cmake
+ pkg-config
+ libssl
+ libfreetype6-dev (Debian)
+ libexpat1-dev (Debian) / libexpat
+ libxcb-composite0-dev (Debian) / libxcb (Arch Linux)

#### Debian 
```rust
sudo apt install cmake
sudo apt install pkg-config libasound2-dev libssl-dev cmake libfreetype6-dev libexpat1-dev libxcb-composite0-dev
```

#### Arch Linux 
```rust
sudo pacman -S cmake pkg-config libexpat libxcb
```

### Launch

The file blk.dat had to be placed inside the ./ins folder and the export are placed inside the ./outs folder if you use the default mode of our application.
The user can change the lauching parameters and can change the folder of import and export.

```rust
cargo run --release i ./dossier_à_lire/
cargo run --release o ./dossier_où_exporter/
cargo run --release io ./dossier_à_lire/ ./dossier_où_exporter/
```

## Implements

### Specifications

The features that's we implement are split into 3 categories :

1. Deserialize the data inside the blk.dat file
    * read the metadata inside the header (version, hash of the previous block,...)
    * read the transactions
    * rebuild the Merkle's tree

2. Analyze the data :
    * count the number of transactions inside a block
    * extract the adresses
    * search the biggest amout of Bitcoin, calculate the average amout of Bitcoin inside a file,...

3. Export the data throught the terminal display or inside a file

### Approach

To realize our project, we've first implemented our parsor so we can read what's inside a blk.dat file one after the other, to delimit the differents fileds and to stock the data extracted inside the structures we've made.
We verify the data integrity with calculating the Merkle's root. After that, we analyze the data from the structure inside the memory.
Finally, the user have a display of the data inside our graphic interface throught a terminal.

### Parsor

The parsor is our main and most important part of our application. At first, it reads all of the blk.dat file inside a buffer, we've chose this solution to move the curser more freely by a certains number of bytes. Then, it will analyze byte after byte the buffer and determine its size, transactions and finally it will stock everything inside the rightful structures to memory.

A blk file is made of blocks one after the other. Each block has multiple fields of size and of differents types. Each field has a fixed size, like the magic bytes who mark the beggining of a block and the type of the network use. This 4 bytes is one of thoses :  f9beb4d9 », « 0b110907 » and « fabfb5da ». To obtain the magic byte, the parsor read simply the 4 first bytes. But, not all the fields have a fixed size. It is not the case of "Input Count", this field determine how much transactions is to come. This number could be one or thousand or even more. The size of the number of transactions that is variable, the parsor will not read the same size each time and have to determine it. How do we determine it ? The input count use a prefix system. If the value of the first byte is more than 0xfc, then we need to check the board below :

   Préfix  | Example             | Description
-----------|---------------------|--------------
 ≤ 0xfc    | 12                  | No more bytes
 ≤ 0xfd    | fd1234              | read 2 more bytes
 ≤ 0xfe    | fe12345678          | read 4 more bytes
 ≤ 0xff    | ff 1234567890abcdef | read 8 more bytes

Our feature "lecture_frac" also do the export of our data to files. It allows to export the all blok.dat file to a json or txt file and become readable. 

{{< gallery match="images/schema/*" sortOrder="asc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=false previewType="blur" embedPreview=true loadJQuery=true >}}

### Other features

+ the feature "conversions" contains a function of conversion between Satoshi and Bitcoin, and a function to convert the timestamp.
+ the feature "merkle_tree" contains the functions useful to rebuild the Merkle's root and to display the all tree.
+ the feature "recherche" (search) offer multiples functions of searching : a function who look for the biggest amount of BTC transfert inside the file, one that can get the same research but for the block, another one that can calculate the volum of exchange of a day.
+ the module "tris" (organize) allows us to organize in O(n log n) every block depending of the timestamp it's got. Let's remind that when we download blocks from the blockchain, it happens that some blocks are not in the chronological order.
+ Finally, the feature "Calculs" contains the functions that allows us to extract the public keys, their hashes, and their adresses.

## Multithreading

After our first use of our parsor, this one was really slow. Generating the Merkle's tree was also really slow. We made some optimization with the help of threads inside lecture_frac and the merkle_tree.

The idea here was that one thread work on a certains amount of blocks. For that, we need to know the number of block inside the file and their size. A block begins with the magic bytes, followed by the block's size. So we need to read the first 8 bytes and to move the cursor. We stock this inside a vector. Then, we need to check that the number of threads is above the number of block, else there is no need to use the threads. It's a simple operation : just divide the number of block per number of threads. If we use the threads, then we give the number of block it needs to work on and where to start on the buffer.