---
title: "Testing the Security of Modbus Services"
date: 2024-03-06T08:56:00.000-08:00
draft: false
type: posts
categories: 
- 
---
# Testing the Security of Modbus Services

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhUjtPDsHTU_40xbA4tMMARNyxJZu0USyUlzoYEpX4CYah14yXlEVLZR6z1N-Hyjiymo4za3D_cURAyM0or3FtLoEONbj-BUp3d3aJJcJxoxgoZmG2n5U93cHXZCaa0mFa0csDUlVtiOl68jsL-58f4rl2wiZCXoQRo-I059Xd76Ekjct9y6vq9JydE1i4/w299-h320/BMS_ModBus.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhUjtPDsHTU_40xbA4tMMARNyxJZu0USyUlzoYEpX4CYah14yXlEVLZR6z1N-Hyjiymo4za3D_cURAyM0or3FtLoEONbj-BUp3d3aJJcJxoxgoZmG2n5U93cHXZCaa0mFa0csDUlVtiOl68jsL-58f4rl2wiZCXoQRo-I059Xd76Ekjct9y6vq9JydE1i4/s569/BMS_ModBus.png)

  

ICS and Building Management Systems (BMS) support several protocols such as _**Modbus**_, _Bacnet_, _Fieldbus_ and so on. Those protocols were designed to provide read/write control over sensors and actuators from a central point. 

Driven by our past experience with BMS, we decided to release our own methodology and internal tool used for proactive attack surface analysis within systems supporting the Modbus protocol.

### The Modbus Protocol

[Modbus](https://en.wikipedia.org/wiki/Modbus) is a well defined protocol described on [modbus.org](https://modbus.org/specs.php). It was created in 1979 and has become one of the most used standards for communication between industrial electronic devices in a wide range of buses and network.

It can be used over a variety of communication media, including serial, TCP, UDP, etc..

The application part of the protocol is quite simple. In particular, the part we are interested into is its Protocol Data Unit, which is independent from the lower layer protocols, is defined as follows:

> | FUNCTION CODE | DATA |

Where _FUNCTION CODE_ is a _1 Byte size 0-127 (0x00-0x7F) value_, and _DATA_ is a sequence of bytes that changes according to the function code.

Here is a set of function codes already defined by the protocol specification:

[![](https://blogger.googleusercontent.com/img/a/AVvXsEiQLA-21SSyWWIjvhqq8WGpypkbuMO8Lk2S2jR8gcPsRaWTNu17-hP15qWqsLp_xivwrZ6LsCC7MUw9xd_VWoxbviIz-IXhnl5uikmO5rcyReEBFT_fUCS8FhdKCSc5MrdCZzNi2XHKu77uklyvKDxZ91rDsVnySjSJDn0-7EURHqQLAHg40DIXXZuQjgk=w640-h434)](https://blogger.googleusercontent.com/img/a/AVvXsEiQLA-21SSyWWIjvhqq8WGpypkbuMO8Lk2S2jR8gcPsRaWTNu17-hP15qWqsLp_xivwrZ6LsCC7MUw9xd_VWoxbviIz-IXhnl5uikmO5rcyReEBFT_fUCS8FhdKCSc5MrdCZzNi2XHKu77uklyvKDxZ91rDsVnySjSJDn0-7EURHqQLAHg40DIXXZuQjgk)

  

By setting a specific function code together with its expected set of data field values, it will be possibile to read/write the status of coils, inputs and registers, or access information about other interesting aspects such as diagnostic data.

For example the following request, queries about the status of 2 coils starting from address 0x0033 in a remote device:

>  \\x01\\x01\\x00\\x33\\x00\\x02  

Where:

> | \\x01 \[**SlaveId**\] | \\x01 \[**Function Code**\] | \\x00\\x33 \[**Address**\] | \\x00\\x02 \[**Quantity**\] |

  

As it can be noticed, that is quite similar to an API based modern application, the name of the function and its arguments:

> _Protocol://URL/**EndPoint**?**Parameters**\=**Values**.._

Apart from the public function codes, several codes are left as _custom implementation_ and are reserved but not defined in the standard.

[![](https://blogger.googleusercontent.com/img/a/AVvXsEjZ23-p_YeYWL_smGaf6c6V9an46z_1Ge0MA9aKlLurjF3xoXaAK427syK0rS9ZeaQ25iUo8izatZkG8YZRF4gALEmVrQgRYItHPC_Q0YtoajS8vdQGr6oD_MvYIYN5tcO1jrn9RhmmCnMgyX4Ii0l33K9BBs-CgxJ2An51det1A2Qc-lLipjKWxyoxGU8=w302-h320)](https://blogger.googleusercontent.com/img/a/AVvXsEjZ23-p_YeYWL_smGaf6c6V9an46z_1Ge0MA9aKlLurjF3xoXaAK427syK0rS9ZeaQ25iUo8izatZkG8YZRF4gALEmVrQgRYItHPC_Q0YtoajS8vdQGr6oD_MvYIYN5tcO1jrn9RhmmCnMgyX4Ii0l33K9BBs-CgxJ2An51det1A2Qc-lLipjKWxyoxGU8)

  

  

In particular _65-72 (0x41-0x48)_, and _100-110 (0x64-0x6e), for a total of 19_ function codes, are left to the vendor/manufacturer for custom implementations.

  
While some of the vendors make the specification of custom functions, publicly available, with all the expected arguments and formats, in their manuals, others do not release any information.  
From a security tester point of view, first questions are:

-   How can we identify if a _**custom**_ Function Code **is implemented** but no details are available?

-   How can we find the correct set of _**expected arguments**_?

-   How can we _**fuzz the arguments**_ to find security issues?

### Modbus Attack Surface

As defined by the standard, if the client request presents some error the slave response will trigger specific exception codes:

> | 0x80 + \[Request Function Code\] | 0xHH \[**_Exception Code_**\] | ... |

Where exceptions code are the following:

[![](https://blogger.googleusercontent.com/img/a/AVvXsEiNhvXjVFuK6QIlpa5EdJvjIyS6oXInfqyPetjnj5yBzlZRq7i-tXFZ5ZcXXEB2s0XUTV5-BZoEk_0-YM4ZCveW3yg8I4n-khbZ3-QDDzrgMAphIgKUTMsmMZfvSlZeCB703j23fc2CtyB8g72oBSSIuRHXc-FulSo5a88TckEToYYvf9rSNGIrnw5S23Y=w640-h523)](https://blogger.googleusercontent.com/img/a/AVvXsEiNhvXjVFuK6QIlpa5EdJvjIyS6oXInfqyPetjnj5yBzlZRq7i-tXFZ5ZcXXEB2s0XUTV5-BZoEk_0-YM4ZCveW3yg8I4n-khbZ3-QDDzrgMAphIgKUTMsmMZfvSlZeCB703j23fc2CtyB8g72oBSSIuRHXc-FulSo5a88TckEToYYvf9rSNGIrnw5S23Y)

  

  

This behavior can help when testing and identifying the exposed services.

In particular, the first three exceptions will help identifying the presence of a custom function code.

-   **0x01 Unimplemented Function**: Function does not exist in the present status.

-   **0x02 Function Implemented but address is not correct**: Function exists but address is wrong.

-   **0x03 Function Implemented but the arguments are not correct**: Function exists but provided arguments are wrong.

As you may already guessed _0x02_ and _0x03_ responses do actually reveal the presence of a custom function!

On the other hand _0x01_ does not mean that there's no custom implementation for that requested function but just that it's not available for the status of the device and it will require some more analysis effort.

  

### The Methodology

Apart from public function codes, where it would be quite easy to check for read/write access to data, we want to identify if there's a set of implemented custom function codes on a black box system.

According to the response, we'll identify if a function code is implemented by analyzing the response for each required function code:

> for code in function\_codes:
> 
>   
> 
> resp = send(code)
> 
> if has\_exception(resp):
> 
>   switch(exception(resp)):
> 
>       case 0x01: # UNIMPLEMENTED Function Error
> 
>                     #Function does not Exist (maybe)!  
> 
>           break;
> 
>       case 0x02: # Invalid Address Error
> 
>                     #Function Exists ! 
> 
>          break;
> 
>       case 0x03: # Invalid Data Error
> 
>                     #Function Exists ! 
> 
>          break;
> 
>       default: # other codes..
> 
>          break;

  

the previous pseudo code shows the approach we use to identify if a custom function is implemented and where we should fuzz.

### The Tool

Here comes [M-SAK (the Modbus Swiss Army Knife)](https://github.com/mindedsecurity/msak), a pretty useful command line and library which can help for scanning and identifying custom functions on a Modbus device.

MSAK is a tool written in Python to help discovering and testing exposed standard and custom services of Modbus Servers/Slaves over Serial or TCP/IP connections. 

  

It also offers a highly customizable payload generator that will help the tester to perform complex scans using a simple but powerful templating format.

  

MSAK can help in:

\- _**finding** undocumented functions_ 

\- **_fuzzing_** _the arguments_ in order to find security issues or weird behavior.

  

For example if we want to scan all function we can just use the Service Scan option, which will scan all functions codes \[1-127\] using the given payload and then will print a summary grouped by response:

  

>  $ python3 msak.py -S -d '0001'
> 
> Requested Data \\x01\\x01\\x00\\x01\\x91\\xD8
> 
> ..
> 
> Requested Data \\x01\\x02\\x00\\x01\\x91\\xD8
> 
> ..
> 
> Requested Data \\x01\\x03\\x00\\x01\\x91\\xD8
> 
> ...
> 
> Requested Data \\x01\\x64\\x00\\x01\\x91\\xD8
> 
> ...
> 
> ILLEGAL DATA VALUE
> 
> 1 (0x01) Read Coils \[FUN\_ID|ADDRESS|TOTAL NUMBER| >BHH\]
> 
> 2 (0x02) Read Discrete Inputs \[FUN\_ID|ADDRESS|TOTAL NUMBER| >BHH\]
> 
> 3 (0x03) Read Holding Registers \[FUN\_ID|ADDRESS|TOTAL NUMBER| >BHH\]
> 
> 4 (0x04) Read Input Registers \[FUN\_ID|ADDRESS|TOTAL NUMBER| >BHH\]
> 
> 15 (0x0F) Write Multiple Coils \[FUN\_ID|ADDRESS|TOTAL NUM|BYTE COUNT|BYTE VALS >BHHBN\*B\]
> 
> 16 (0x10) Write Multiple registers \[FUN\_ID|ADDRESS|TOTAL NUM|BYTE COUNT|VALS >BHHBN\*H\]
> 
> 20 (0x14) Read File Record
> 
> ACCEPTED\_WITH\_RESPONSE
> 
> 5 (0x05) Write Single Coil \[FUN\_ID|ADDRESS|COIL VALUE| >BHH\]
> 
> 6 (0x06) Write Single Register \[FUN\_ID|ADDRESS|REG VALUE| >BHH\]
> 
> 17 (0x11) Report Server ID (Serial Line only)  \[FUN\_ID >B\]
> 
> 105 CUSTOM
> 
> ILLEGAL FUNCTION
> 
> 7 (0x07) Read Exception Status (Serial Line only) \[FUN\_ID >B\]
> 
> 8 (0x08) Diagnostics (Serial Line only) \[|FUN\_ID|SUB\_FUN|VALUES| >BHN\*H\]
> 
> 9 CUSTOM
> 
> 10 CUSTOM
> 
> 11 (0x0B) Get Comm Event Counter (Serial Line only) \[FUN\_ID >B\]
> 
> 12 (0x0C) Get Comm Event Log (Serial Line only)\[FUN\_ID >B\]
> 
> 13 CUSTOM
> 
> ....
> 
> ILLEGAL DATA ADDRESS
> 
> 21 (0x15) Write File Record

  

The result shows that a custom function 0x69 (105) was found as the device responded with a 0x03 exception (Illegal data value). We can now try to find the correct set of arguments through fuzzing using the following command: 

  

> $ python3 msak.py -C -d '0169{R\[0,0xFF,">H"\]}'

  

Which will send 0 to 65535 requests and collect the responses.

In fact:

\- 0x01 is the slave ID

\- 0x69 is the service function

\- R\[0,0xFF,">H"\] asks to generate 0-65535 sequence of payloads in for a 2 Bytes, little endian format.

\- the request will be via serial port and the CRC are automatically computed.

  

After the whole scan, MSAK will return an output similar to the following:

  

> {
> 
> 'NO\_RESPONSE':
> 
>   \[ ... 
> 
>    b'\\x01\\x69\\x36\\xfe',
> 
>   ...
> 
>   \], 
> 
> 'ACCEPTED\_WITH\_RESPONSE': 
> 
>   \[ b'\\x01\\x69\\x36\\xff', 
> 
>     b'\\x01\\x69\\x37\\x00',
> 
> ...
> 
>   \]
> 
> }
> 
>   

  

We have found that the undocumented function responds to arguments values of \\x36\\xff and \\x37\\x00! 

Next step is to play with this new function and see if there's some way to abuse it...

What could go wrong if an undocumented function for firmware update is found, right?! :P 

  

For more information, the fuzzing engine support several template patterns documented on MSAK [README](https://github.com/mindedsecurity/msak?tab=readme-ov-file#custom-scan) file.

  

N.B.: The fuzzing template engine is available also as a separate python library called [Simple Payload Generator](https://github.com/mindedsecurity/simple_payload_generator).

  

### Conclusions

Although Modbus is a quite old protocol, it's still used on Build Management Systems, Industrial Control Systems and SCADA Systems. 

Apart from dealing using well defined functionalities, to  read/write sensors and controls, which could lead to very interesting security issues, the standard leaves pretty much space to vendors for implementing their own services and that might be even more interesting from a security point of view!

That's where MSAK can give its best by automating the boring part and leave all the fun to the tester!

  

Feel free to send us your feedback and happy hacking!

  

**Author**: Stefano Di Paola 

**Twitter**: [@WisecWisec](https://twitter.com/wisecwisec)

#### [Source](https://blog.mindedsecurity.com/feeds/3003270381575270709/comments/default)

<br/>
---
