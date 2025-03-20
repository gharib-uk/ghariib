---
title: "Uncovering GStreamer secrets"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

In this post, I’ll walk you through the vulnerabilities I uncovered in the GStreamer library and how I built a custom fuzzing generator to target MP4 files.

The post Uncovering GStreamer secrets appeared first on The GitHub Blog.

In this blog post, I’ll show the results of my recent security research on GStreamer, the open source multimedia framework at the core of GNOME’s multimedia functionality.

I’ll also go through the approach I used to find some of the most elusive vulnerabilities, generating a custom input corpus from scratch to enhance fuzzing results.

## GStreamer

GStreamer is an open source multimedia framework that provides extensive capabilities, including audio and video decoding, subtitle parsing, and media streaming, among others. It also supports a broad range of codecs, such as MP4, MKV, OGG, and AVI.

GStreamer is distributed by default on any Linux distribution that uses GNOME as the desktop environment, including Ubuntu, Fedora, and openSUSE. It provides multimedia support for key applications like Nautilus (Ubuntu’s default file browser), GNOME Videos, and Rhythmbox. It’s also used by tracker-miners, the Ubuntu’s metadata indexer–an application that my colleague, Kev, was able to exploit last year.

This makes GStreamer a very interesting target from a security perspective, as critical vulnerabilities in the library can open numerous attack vectors. That’s why I picked it as a target for my security research.

It’s worth noting that GStreamer is a large library that includes more than 300 different sub-modules. For this research, I decided to focus on only the “Base” and “Good” plugins, which are included by default in the Ubuntu distribution.

## Results

During my research I found a total of 29 new vulnerabilities in GStreamer, most of them in the MKV and MP4 formats.

Below you can find a summary of the vulnerabilities I discovered:

| GHSL | CVE | DESCRIPTION |
| --- | --- | --- |
| GHSL-2024-094 | CVE-2024-47537 | OOB-write in isomp4/qtdemux.c |
| GHSL-2024-115 | CVE-2024-47538 | Stack-buffer overflow in vorbis\_handle\_identification\_packet |
| GHSL-2024-116 | CVE-2024-47607 | Stack-buffer overflow in gst\_opus\_dec\_parse\_header |
| GHSL-2024-117 | CVE-2024-47615 | OOB-Write in gst\_parse\_vorbis\_setup\_packet |
| GHSL-2024-118 | CVE-2024-47613 | OOB-Write in gst\_gdk\_pixbuf\_dec\_flush |
| GHSL-2024-166 | CVE-2024-47606 | Memcpy parameter overlap in qtdemux\_parse\_theora\_extension leading to OOB-write |
| GHSL-2024-195 | CVE-2024-47539 | OOB-write in convert\_to\_s334\_1a |
| GHSL-2024-197 | CVE-2024-47540 | Uninitialized variable in gst\_matroska\_demux\_add\_wvpk\_header leading to function pointer ovewriting |
| GHSL-2024-228 | CVE-2024-47541 | OOB-write in subparse/gstssaparse.c |
| GHSL-2024-235 | CVE-2024-47542 | Null pointer dereference in id3v2\_read\_synch\_uint |
| GHSL-2024-236 | CVE-2024-47543 | OOB-read in qtdemux\_parse\_container |
| GHSL-2024-238 | CVE-2024-47544 | Null pointer dereference in qtdemux\_parse\_sbgp |
| GHSL-2024-242 | CVE-2024-47545 | Integer underflow in FOURCC\_strf parsing leading to OOB-read |
| GHSL-2024-243 | CVE-2024-47546 | Integer underflow in extract\_cc\_from\_data leading to OOB-read |
| GHSL-2024-244 | CVE-2024-47596 | OOB-read in FOURCC\_SMI\_ parsing |
| GHSL-2024-245 | CVE-2024-47597 | OOB-read in qtdemux\_parse\_samples |
| GHSL-2024-246 | CVE-2024-47598 | OOB-read in qtdemux\_merge\_sample\_table |
| GHSL-2024-247 | CVE-2024-47599 | Null pointer dereference in gst\_jpeg\_dec\_negotiate |
| GHSL-2024-248 | CVE-2024-47600 | OOB-read in format\_channel\_mask |
| GHSL-2024-249 | CVE-2024-47601 | Null pointer dereference in gst\_matroska\_demux\_parse\_blockgroup\_or\_simpleblock |
| GHSL-2024-250 | CVE-2024-47602 | Null pointer dereference in gst\_matroska\_demux\_add\_wvpk\_header |
| GHSL-2024-251 | CVE-2024-47603 | Null pointer dereference in gst\_matroska\_demux\_update\_tracks |
| GHSL-2024-258 | CVE-2024-47778 | OOB-read in gst\_wavparse\_adtl\_chunk |
| GHSL-2024-259 | CVE-2024-47777 | OOB-read in gst\_wavparse\_smpl\_chunk |
| GHSL-2024-260 | CVE-2024-47776 | OOB-read in gst\_wavparse\_cue\_chunk |
| GHSL-2024-261 | CVE-2024-47775 | OOB-read in parse\_ds64 |
| GHSL-2024-262 | CVE-2024-47774 | OOB-read in gst\_avi\_subtitle\_parse\_gab2\_chunk |
| GHSL-2024-263 | CVE-2024-47835 | Null pointer dereference in parse\_lrc |
| GHSL-2024-280 | CVE-2024-47834 | Use-After-Free read in Matroska CodecPrivate |

## Fuzzing media files: The problem

Nowadays, coverage-guided fuzzers have become the “de facto” tools for finding vulnerabilities in C/C++ projects. Their ability to discover rare execution paths, combined with their ease of use, has made them the preferred choice among security researchers.

The most common approach is to start with an initial input corpus, which is then successively mutated by the different mutators. The standard method to create this initial input corpus is to gather a large collection of sample files that provide a good representative coverage of the format you want to fuzz.

But with multimedia files, this approach has a major drawback: media files are typically very large (often in the range of megabytes or gigabytes). So, using such large files as the initial input corpus greatly slows down the fuzzing process, as the fuzzer usually goes over every byte of the file.

There are various minimization approaches that try to reduce file size, but they tend to be quite simplistic and often yield poor results. And, in the case of complex file formats, they can even break the file’s logic.

It’s for this reason that for my GStreamer fuzzing journey, I opted for “generating” an initial input corpus from scratch.

## The alternative: corpus generators

An alternative to gathering files is to create an input corpus from scratch. Or in other words, without using any preexisting files as examples.

To do this, we need a way to transform the target file format into a program that generates files compliant with that format. Two possible solutions arise:

1. Use a grammar-based generator. This category of generators makes use of formal grammars to define the file format, and subsequently generate the input corpus. In this category, we can mention tools like Grammarinator, an open source grammar-based fuzzer that creates test cases according to an input ANTLR v4 grammar. In this past blog post, I also explained how I used AFL++ Grammar-Mutator for fuzzing Apache HTTP server.
2. To create a generator specifically for the target software. In this case, we rely on analyzing how the software parses the file format to create a compatible input generator.

Of course, the second solution is more time-consuming, as we need not only to understand the file format structure but also to analyze how the target software works.

But at the same time, it solves two problems in one shot:

- On one hand, we’ll generate much smaller files, drastically speeding up the fuzzing process speed.
- On the other hand, these “custom” files are likely to produce better code coverage and potentially uncover more vulnerabilities.

This is the method I opted for and it allowed me to find some of the most interesting vulnerabilities in the MP4 and MKV parsers–vulnerabilities that until then, had not been detected by the fuzzer.

## Implementing an input corpus generator for MP4

In this section, I will explain how I created an input corpus generator for the MP4 format. I used the same approach for fuzzing the MKV format as well.

### MP4 format

To start, I will show a brief description of the MP4 format.

MP4, officially known as MPEG-4 Part 14, is one of the most widely used multimedia container formats today, due to its broad compatibility and widespread support across various platforms and devices. It supports packaging of multiple media types such as video, audio, images, and complex metadata.

MP4 is basically an evolution of Apple’s QuickTime media format, which was standardized by ISO as MPEG-4. The .mp4 container format is specified by the “MPEG-4 Part 14: MP4 file format” section.

MP4 files are structured as a series of “boxes” (or “atoms”), each containing specific multimedia data needed to construct the media. Each box has a designated type that describes its purpose.

These boxes can also contain other nested boxes, creating a modular and hierarchical structure that simplifies parsing and manipulation.

Each **box/atom** includes the following fields:

- **Size**: A 32-bit integer indicating the total size of the box in bytes, including the header and data.
- **Type**: A 4-character code (FourCC) that identifies the box’s purpose.
- **Data**: The actual content or payload of the box.

Some boxes may also include:

- **Extended size**: A 64-bit integer that allows for boxes larger than 4GB.
- **User type**: A 16-byte (128-bit) UUID that enables the creation of custom boxes without conflicting with standard types.

![Mp4 box structure](https://github.blog/wp-content/uploads/2024/12/image1-4.png?w=730&resize=730%2C574)

An MP4 file is typically structured in the following way:

- **ftyp** (File Type Box): Indicates the file type and compatibility.
- **mdat** (Media Data Box): Contains the actual media data (for example, audio and video frames).
- **moov** (Movie Box): Contains metadata for the entire presentation, including details about tracks and their structures:
- **trak** (Track Box): Represents individual tracks (for example, video, audio) within the file.
- **udta** (User Data Box): Stores user-defined data that may include additional metadata or custom information.

![Common MP4 file structure](https://github.blog/wp-content/uploads/2024/12/image2-3.png?w=1024&resize=1024%2C441)

Once we understand how an MP4 file is structured, we might ask ourselves, “Why are fuzzers not able to successfully mutate an MP4 file?”

To answer this question, we need to take a look at how coverage-guided fuzzers mutate input files. Let’s take AFL–one of the most widely used fuzzers out there–as an example. AFL’s default mutators can be summarized as follows:

- Bit/Bytes mutators: These mutators flip some bits or bytes within the input file. They don’t change the file size.
- Block insertion/deletion: These mutators insert new data blocks or delete sections from the input file. They modify the file size.

The main problem lies in the latter category of mutators. As soon as the fuzzer modifies the data within an mp4 box, the size field of the box should be also updated to reflect the new size. Furthermore, if the size of a box changes, the size fields of all its parent boxes must also be recalculated and updated accordingly.

Implementing this functionality as a simple mutator can be quite complex, as it requires the fuzzer to track and update the implicit structure of the MP4 file.

### Generator implementation

The algorithm I used for implementing my generator follows these steps:

#### Step 1: Generating unlabelled trees

Structurally, an MP4 file can be visualized as a tree-like structure, where each node corresponds to an MP4 box. Thus, the first step in our generator implementation involves creating a set of unlabelled trees.

In this phase, we create trees with empty nodes that do not yet have a tag assigned. Each node represents a potential MP4 box. To make sure we have a variety of input samples, we generate trees with various structures and different node counts.

![3 different 9-node unlabelled trees](https://github.blog/wp-content/uploads/2024/12/image3-3.png?w=1024&resize=1024%2C255)

In the following code snippet, we see the constructor of the `RandomTree class`, which generates a random tree structure with a specified total nodes (`total_nodes`):

```cpp
RandomTree::RandomTree(uint32_t total_nodes){
uint32_t curr_level = 0;

//Root node
new_node(-1, curr_level);
curr_level++;

uint32_t rem_nodes = total_nodes - 1;
uint32_t current_node = 0;

while(rem_nodes > 0){

uint32_t num_children = rand_uint32(1, rem_nodes);
uint32_t min_value = this->levels[curr_level-1].front();
uint32_t max_value = this->levels[curr_level-1].back();

for(int i=0; i<num_children; i++){
uint32_t parent_id = rand_uint32(min_value, max_value);
new_node(parent_id, curr_level);
}

curr_level++;
rem_nodes -= num_children;
}
}
```

This code traverses the tree level by level (Level Order Traversal), adding a random number (`rand_uint32`) of children nodes (`num_children`). This approach of assigning a random number of child nodes to each parent node will generate highly diverse tree structures.

![Random generation of child nodes](https://github.blog/wp-content/uploads/2024/12/Image3_2.png?w=1024&resize=1024%2C456)

After all children are added for the current level, `curr_level` is incremented to move to the next level.

Once `rem_nodes` is 0, the `RandomTree` generation is complete, and we move on to generate another new `RandomTree`.

#### Step 2: Assigning tags to nodes

Once we have a set of unlabelled trees, we proceed to assign random `tags` to each node.

These tags correspond to the four-character codes (FOURCCs) used to identify the types of MP4 boxes, such as `moov`, `trak`, or `mdat`.

In the following code snippet, we see two different `fourcc_info` structs: `FOURCC_LIST` which represents the leaf nodes of the tree, and `CONTAINER_LIST` which represents the rest of the nodes.

The `fourcc_info` struct includes the following fields:

- _fourcc_: A 4-byte FourCC ID
- _description_: A string describing the FourCC
- _minimum\_size_: The minimum size of the data associated with this FourCC

```cpp
const fourcc_info CONTAINER_LIST[] = {

{FOURCC_moov, "movie", 0,},
{FOURCC_vttc, "VTTCueBox 14496-30", 0},
{FOURCC_clip, "clipping", 0,},
{FOURCC_trak, "track", 0,},
{FOURCC_udta, "user data", 0,},
…

const fourcc_info FOURCC_LIST[] = {

{FOURCC_crgn, "clipping region", 0,},
{FOURCC_kmat, "compressed matte", 0,},
{FOURCC_elst, "edit list", 0,},
{FOURCC_load, "track load settings", 0,},
```

Then, the `MP4_labeler` constructor takes a `RandomTree` instance as input, iterates through its nodes, and assigns a label to each node based on whether it is a leaf (no children) or a container (has children):

```cpp
…

MP4_labeler::MP4_labeler(RandomTree *in_tree) {
…
for(int i=1; i < this->tree->size(); i++){

Node &node = this->tree->get_node(i);
…
if(node.children().size() == 0){
//LEAF
uint32_t random = rand_uint32(0, FOURCC_LIST_SIZE-1);
fourcc = FOURCC_LIST[random].fourcc;
…
}else{
//CONTAINER
uint32_t random = rand_uint32(0, CONTAINER_LIST_SIZE-1);
fourcc = CONTAINER_LIST[random].fourcc;
…
}
…
node.set_label(label);
}
}
```

After this stage, all nodes will have an assigned tag:

![Labeled trees with MP4 box tags](https://github.blog/wp-content/uploads/2024/12/image4-3.png?w=811&resize=811%2C1024)

#### Step 3: Adding random-size data fields

The next step is to add a random-size data field to each node. This data simulates the content within each MP4 box.  
In the following code, at first we set the minimum size (`min_size`) of the padding specified in the selected `fourcc_info` from `FOURCC_LIST`. Then, we append `padding` number of null bytes (x00) to the label:

```cpp
if(node.children().size() == 0){
//LEAF
…
padding = FOURCC_LIST[random].min_size;
random_data = rand_uint32(4, 16);
}else{
//CONTAINER
…
padding = CONTAINER_LIST[random].min_size;
random_data = 0;
}
…
std::string label = uint32_to_string(fourcc);
label += std::string(padding, 'x00');
label += std::string(random_data, 'x41');
```

By varying the data sizes, we make sure the fuzzer has sufficient space to inject data into the box data sections, without needing to modify the input file size.

#### Step 4: Calculating box sizes

Finally, we calculate the size of each box and recursively update the tree accordingly.

The `traverse` method recursively traverses the tree structure serializing the node data and calculating the resulting size box (`size)`. Then, it propagates size updates up the tree (`traverse(child)`) so that parent boxes include the sizes of their child boxes:

```cpp
std::string MP4_labeler::traverse(Node &node){
…
for(int i=0; i < node.children().size(); i++){ Node &child = tree->get_node(node.children()[i]);

output += traverse(child);
}

uint32_t size;
if(node.get_id() == 0){
size = 20;
}else{
size = node.get_label().size() + output.size() + 4;
}

std::string label = node.get_label();
uint32_t label_size = label.size();

output = uint32_to_string_BE(size) + label + output;
…
}
```

The number of generated input files can vary depending on the time and resources you can dedicate to fuzzing. In my case, I generated an input corpus of approximately 4 million files.

### Code

You can find my C++ code example here.

## Acknowledgments

A big thank you to the GStreamer developer team for their collaboration and responsiveness, and especially to Sebastian Dröge for his quick and effective bug fixes.

I would also like to thank my colleague, Jonathan Evans, for managing the CVE assignment process.

## References

- https://www.mpeg.org/standards/MPEG-4/14/
- https://gstreamer.freedesktop.org/

The post Uncovering GStreamer secrets appeared first on The GitHub Blog.

Go to Source
