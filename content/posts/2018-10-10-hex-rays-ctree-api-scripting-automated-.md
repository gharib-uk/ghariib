---
title: "Hex-Rays CTREE API Scripting Automated Contextual Function Renaming"
date: Wed, 10 Oct 2018 03:19:28 +0000
draft: false
type: posts
categories: 
- 
---
# Hex-Rays CTREE API Scripting Automated Contextual Function Renaming

<br/>

<br/>
One thing I teach my students in my [static reverse engineering training classes](https://www.msreverseengineering.com/training-classes) is to exploit information that programmers have left in the binaries for debugging purposes. The most obvious example of this is when the program contains a debug logging function, where one of the parameters is the function's name. In this case, the reverse engineer can write a script to extract the function names and automatically rename the calling function, thus saving valuable time and making their lives easier. (I teach several other example scenarios.)

Inspired by [my recent forays into the Hex-Rays API](http://www.msreverseengineering.com/blog/2018/9/19/hex-rays-microcode-api-vs-obfuscating-compiler), today I found a new method for exploiting such ad hoc debug-type information. This entry will demonstrate how to use the Hex-Rays CTREE API to extract information and automatically rename functions as a result, which would otherwise be more cumbersome and error-prone to do with the ordinary IDA SDK. The code is available [here](https://github.com/RolfRolles/Miscellaneous/blob/master/RenameBySwitchCaseName.py).

In particular, today I was reverse engineering the Hex-Rays decompiler itself. The Hex-Rays API is unusual in that the user-accessible API functions are not defined as exports from hexrays.dll. (In fact, hexrays.dll exports only the standard DLL entrypoint, as well as the standard plugin\_t structure required of all IDA plugins.) Instead, the "exported" API methods are all declared as small inline functions that each invoke the same function, namely "hexdsp", which serves as a central dispatcher to the public Hex-Rays API functions. The first parameter to hexdsp is an enumeration element of type "hexcall\_t", which tells the Hex-Rays kernel which API is being called, and the subsequent parameters (of which there are a variable number) are specific to the called API. (Internally to hexrays.dll, the hexdsp function performs a switch over the hexcall\_t parameter, and then invokes the specified function.)

To be concrete about it, the last 3500 lines of hexrays.hpp all look roughly like this:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139759098-2RO1SZ23WJ0WM8OIF6O6/HexDsp.png?format=1000w)

If the API functions had been exported from the DLL, IDA would have automatically renamed them for us, which would have made trivial the task of finding the APIs that we wanted to reverse engineer. Instead, nearly none of the functions in the the database have meaningful names.

The lack of meaningful function names does not prove to be a huge impediment to reverse engineering Hex-Rays API functions. We simply need to locate the hexdsp function in hexrays.dll, and locate the switch case that corresponds to the hexcall\_t enumeration element that interests us. Most of these switch cases are one-liners that immediately invoke the relevant API function.

To find hexdsp, we can textually search the disassembly listing for the word "switch", which IDA inserts automatically as a comment upon encountering a compiled switch construct.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139786267-VNBWA6R9FS6UPZZZQB0Q/TextSearch.png?format=1000w)

Next, we can filter the search by the word "cases" to obtain the beginning of each switch construct, as well as the number of cases that the switch contains.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139807220-9SX5Z09ST6LFNVZRL06K/FilteredSearch.png?format=1000w)

Finally, we can count the number of enumeration elements in hexcall\_t, and look for a switch with roughly that many cases. (For this, I simply selected the contents of the hexcall\_t enumeration in my text editor, and looked at the "lines selected" display at the bottom.) hexcall\_t contains 532 enumeration elements, and one of the switches has exactly 532 cases. That must be hexdsp.

Now, to reverse engineer any particular API function, we just need to find the value of the associated constant in the hexcall\_t enumeration, and examine that particular switch case. IDA can offer us some more assistance here, also. I begin by copying and pasting the hexcall\_t enumeration into its own text file and saving it:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139834179-XBYN141KNFQLRLJRUWJ6/hexcall_t_C.png?format=1000w)

Next, I use IDA's C header parsing functionality to automatically create an enumeration that I can use inside of IDA. If successful, we get a message box telling us so.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139856925-KQW2Z3PB08J2SK9EU16Y/ParseCHeaderFile.png?format=1000w)

In IDA's Enumerations window, now we need to add the enumeration we just imported. Press insert to bring up the "Add enum type" window, and then use the "Add standard enum by enum name" option near the bottom. It will bring up a list of all currently-known enumerations; the one we just imported will be at the bottom.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139875391-NMUXAWP3CSBOUS0REC34/LoadEnum.png?format=1000w)

We can use this enumeration to make the decompilation listing prettier. The decompiled switch statement currently shows the hexcall\_t enumeration elements as integers. We can ask Hex-Rays to display them as enumeration element names instead, by placing our cursor on one of the integers and pressing 'M'.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139901878-16HNBIWJP7EDXVQ5TKYM/HRApplyEnum.png?format=1000w)

Select the hexcall\_t entry from the dialog that pops up and press enter. Voila, now we see the enumerated names in the decompilation listing:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139926799-QIHAVOAMM251DR7NEFOK/HREnumApplied.png?format=1000w)

At this point, locating particular Hex-Rays API functions is pretty easy. The switch now uses human-readable names for the enumeration elements; we just find the one we're interested in, and look at the function called in that switch case. To wit, here's gen\_microcode(), the one I had been after in the first place:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139947584-MQ685MFVOVBSKM622DFM/SwitchPrettyNames.png?format=1000w)

Of course, finding the correct switch case is still tedious, given that there are 532 of them. I thought that the nicest solution would be to rename the functions after the name of the hexcall\_t enumeration element in the corresponding switch case. Of course, doing this by hand for all 532 hexcall\_t enumeration elements would be a waste of time, and we should automate it instead.

For this task, I decided to use the Hex-Rays CTREE API via IDAPython, which I discussed somewhat in [my recent blog entry about the Hex-Rays Microcode API](http://www.msreverseengineering.com/blog/2018/9/19/hex-rays-microcode-api-vs-obfuscating-compiler). My observation was that we could extract the case number from the case statements, get the name of the pertinent enumeration element, extract the address of the called function from the first line of code in the case body (if the first line does call a function), and then rename the called address after the enumeration element. Using the Hex-Rays API saves us from having to write nasty, brittle code to search the disassembly listing instruction-by-instruction, and also makes it convenient for us to access the switch case numbers and their associated case statement bodies.

Our script will make use of so-called "CTREE visitor" objects. Our CTREE visitor classes contain methods such as "visit\_insn" and "visit\_expr", which Hex-Rays will invoke for every element in the CTREE listing.

The first visitor, which I called SwitchExaminer, exists merely to locate the switch statement within hexdsp's decompilation listing. Once found, it iterates over each switch case. For each case, it first extracts the case number. Next, it then extracts the first line of code in the body of the case statement, and passes it to another visitor. The second visitor, called a CallExtractor, extracts the addresses of all called functions (if any).

Finally, some glue code at the bottom of the script applies the visitors to the decompilation of hexdsp, and then renames the called functions after the hexcall\_t enumerated element numbers that were collected from the switch statement.

That's all; after about 100 lines of heavily-commented IDAPython code, the script renames 436 functions for us automatically, nearly 10% of the non-library, non-thunk functions in the binary.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1539139972823-J8DT84KILCOVFNGTJ6OX/DecompRenamed.png?format=1000w)

You can find the code [here](https://github.com/RolfRolles/Miscellaneous/blob/master/RenameBySwitchCaseName.py). A small note is that the code takes advantage of an IDAPython wrapper function regarding switch case values, which exists in IDA 7.2 beta, but does not exist in IDA 7.1 or below. In particular, you will not be able to run the script as-is until IDA 7.2 is released. If you would like to do something similar with a different use case that does not require you to extract switch case label values, you might be able to adapt the existing code for that purpose in the meantime.

#### [Source](https://www.msreverseengineering.com/blog/2018/10/9/hex-rays-ctree-api-scripting-automated-contextual-function-renaming)

<br/>
---
