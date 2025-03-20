---
title: "BlackPwn BlackPhone SilentText Type Confusion Vulnerability"
date: Sun, 28 Dec 2014 03:44:00 +0000
draft: false
type: posts
---
# BlackPwn BlackPhone SilentText Type Confusion Vulnerability

<br/>

<br/>
Privacy is a hot topic at the moment - it continues to dominate the headlines as news of new NSA incursions, celebrity phone hacks, and corporate breaches are being reported on an increasingly regular basis. In response to this, a number of products have been brought to market that attempt to provide consumers with a greater level of privacy than typical devices allow for. In the phone market, one of the premier products to be released in recent years is undoubtedly the BlackPhone ([http://www.blackphone.ch](http://www.blackphone.ch/)), which has been cited numerous times in tech publications as being one of the best available defenses against mass surveillance, as it provides full end-to-end encryption facilities for voice calls and text/MMS messaging.  
  
While exploring my recently purchased BlackPhone, I discovered that the messaging application contains a serious memory corruption vulnerability that can be triggered remotely by an attacker.  If exploited successfully, this flaw could be used to gain remote arbitrary code execution on the target's handset. The code run by the attacker will have the privileges of the messaging application, which is a standard Android application with some additional privileges. Specifically, it is possible to:  
 

-   decrypt messages / commandeer SilentCircle account
-   gather location information
-   read contacts
-   write to external storage
-   run additional code of the attacker's choosing (such as a privilege escalation exploit aimed at gaining root or kernel-mode access, thus taking complete control of the phone)

  
The only knowledge required by the attacker is the target's Silent Circle ID or phone number - the target does not need to be lured in to contacting the attacker (although the flaw is exploitable in this scenario as well).  
  
This issue is now patched by both Silent Circle and Blackphone in the respective App Stores / Product updates.  
  
The remainder of this post discusses the technical details of this vulnerability, citing the source code of the vulnerable application where appropriate. This code is available from Silent Circle's github repository ([https://github.com/SilentCircle](https://github.com/SilentCircle)).

SilentText Messaging Application
--------------------------------

The SilentText application bundled with Blackphone (and also made available as a standalone app for Android and iPhone) provides the ability for users to send text messages and share files over an encrypted channel. This encrypted channel is established and managed using the 'Silent Circle Instant Message Protocol' (SCIMP), which is tunneled over Silent Circle's XMPP servers. SCIMP provides end-to-end encryption, so that data exchanged in a given conversation cannot be decrypted by an eavesdropping third party (including Silent Circle). The SCIMP implementation supplied with SilentText contains a type confusion vulnerability, that allows an attacker to directly overwrite a pointer in memory (either partially or in full), which when successfully exploited can be used to gain remote, unauthenticated access to the vulnerable device.  
  
Before discussing the vulnerability itself, a quick overview of SCIMP and YAJL (Yet Another JSON Library - a third party library relevant to the flaw) is provided.

The SCIMP Protocol
------------------

SCIMP is a simple message-oriented protocol, where messages are encoded as JSON objects, and then sent over XMPP. SCIMP messages are distinguished by a fixed header string "?SCIMP:", followed by a base64-encoded JSON object, followed by a terminator ("."). An example message looks like this.  
  
?SCIMP:ewogICAgImNvbW1pdCI6IHsKICAgICAgICAidmV  
yc2lvbiI6IDEsCiAgICAgICAgImNpcGhlclN1aXRlIjogM  
SwKICAgICAgICAic2FzTWV0aG9kIjogMSwKCSJIcGtpIjog  
IkhFZ3JXRkVj/eUloM1psVkVNeUlSemhOeVByb1hrUXBrRm  
JLVTRjdjBtajg9IgogICAgfQp9Cg==.  
  
  
SCIMP messages have a message type, followed by a number of data fields depending on the message type. Message type can be one of the following:

-   commit - sent by the initiator wishing to establish a new session. Also can be used to re-key an existing session.
-   dh1 - sent by the remote party (responder) in response to a commit message as part of session establishment.
-   dh2 - sent by the initiator in response to a dh1 message as part of session establishment.
-   confirm - sent by the responder in response to a dh2 message indicating that session establishment was successful.
-   data - application-level data sent after a secure session has been established.

  
These messages are encoded in JSON using a single map object with the name of the map indicating the message type, and a variable number of string or integer-based variables within the map relevant to the specific message type. A JSON-encoded SCIMP message is shown.

> {  
> "commit": {  
>          "version": 1,  
>          "cipherSuite": 1,  
>          "sasMethod": 1,  
>          "Hpki": "HEgrWFEcyIh3ZlVEMyIRzhNyProXkQpkFbKU4cv0mj8="  
> }  
> }

  
The JSON serialization and de-serialization is handled by a third-party library named "Yet Another JSON Library", or libyajl. The source code for this library is available at [http://lloyd.github.com/yajl](http://lloyd.github.com/yajl). Understanding the basics of this API is relevant to the discovered SCIMP vulnerability, and so is briefly covered here.

JSON Parsing - The YAJL API
---------------------------

  
The YAJL library is initialized with a call to **yajl\_alloc()**, which has the following prototype.

> yajl\_handle  
> yajl\_alloc(const yajl\_callbacks \* callbacks, yajl\_alloc\_funcs \* afs,  
>  void \* ctx);

  
This function creates an opaque yajl\_handle that is later passed as a parameter to **yajl\_parse()**, the function responsible for parsing a block of JSON text. The first parameter of the **yajl\_alloc()** function is a yajl\_callbacks structure, which allows the caller to define a series of callback functions that will be invoked during JSON parsing when certain elements are encountered. The **yajl\_callback** structure is as follows.

>     /\*\* yajl is an event driven parser.  this means as json elements are  
>      \*  parsed, you are called back to do something with the data.  The  
>      \*  functions in this table indicate the various events for which  
>      \*  you will be called back.  Each callback accepts a "context"  
>      \*  pointer, this is a void \* that is passed into the yajl\_parse  
>      \*  function which the client code may use to pass around context.  
>      \*  
>      \*  All callbacks return an integer.  If non-zero, the parse will  
>      \*  continue.  If zero, the parse will be canceled and  
>      \*  yajl\_status\_client\_canceled will be returned from the parse.  
>      \*  
>      \*  \\attention {  
>      \*    A note about the handling of numbers:  
>      \*  
>      \*    yajl will only convert numbers that can be represented in a  
>      \*    double or a 64 bit (long long) int.  All other numbers will  
>      \*    be passed to the client in string form using the yajl\_number  
>      \*    callback.  Furthermore, if yajl\_number is not NULL, it will  
>      \*    always be used to return numbers, that is yajl\_integer and  
>      \*    yajl\_double will be ignored.  If yajl\_number is NULL but one  
>      \*    of yajl\_integer or yajl\_double are defined, parsing of a  
>      \*    number larger than is representable in a double or 64 bit  
>      \*    integer will result in a parse error.  
>      \*  }  
>      \*/  
>     typedef struct {  
>         int (\* yajl\_null)(void \* ctx);  
>         int (\* yajl\_boolean)(void \* ctx, int boolVal);  
>         int (\* yajl\_integer)(void \* ctx, long long integerVal);  
>         int (\* yajl\_double)(void \* ctx, double doubleVal);  
>         /\*\* A callback which passes the string representation of the number  
>          \*  back to the client.  Will be used for all numbers when present \*/  
>         int (\* yajl\_number)(void \* ctx, const char \* numberVal,  
>                             size\_t numberLen);  
>         /\*\* strings are returned as pointers into the JSON text when,  
>          \* possible, as a result, they are \_not\_ null padded \*/  
>         int (\* yajl\_string)(void \* ctx, const unsigned char \* stringVal,  
>                             size\_t stringLen);  
>         int (\* yajl\_start\_map)(void \* ctx);  
>         int (\* yajl\_map\_key)(void \* ctx, const unsigned char \* key,  
>                              size\_t stringLen);  
>         int (\* yajl\_end\_map)(void \* ctx);  
>         int (\* yajl\_start\_array)(void \* ctx);  
>         int (\* yajl\_end\_array)(void \* ctx);  
>     } yajl\_callbacks;

  
 **ctx****ctx** parameter passed by the called as the third parameter to**yajl\_alloc()**, and can be anything the caller wishes.  
  
Finally, it is possible for the caller to specify a custom allocator from which to allocate blocks of memory used to store JSON strings and so on. This can be achieved by filling out the second parameter to the**yajl\_alloc()** function with callbacks to custom allocation and free routines. The **yajl\_alloc\_funcs** structure that encapsulates these callbacks is defined as follows.

> /\*\* A structure which can be passed to yajl\_\*\_alloc routines to allow the  
> \*  client to specify memory allocation functions to be used. \*/  
> typedef struct  
> {   
>     /\*\* pointer to a function that can allocate uninitialized memory \*/  
>     yajl\_malloc\_func malloc;  
>     /\*\* pointer to a function that can resize memory allocations \*/  
>     yajl\_realloc\_func realloc;  
>     /\*\* pointer to a function that can free memory allocated using  
>      \*  reallocFunction or mallocFunction \*/  
>     yajl\_free\_func free;  
>     /\*\* a context pointer that will be passed to above allocation routines \*/  
>     void \* ctx;  
> } yajl\_alloc\_funcs;

  
After a handle is allocated, a block of JSON text can be parsed by calling yajl\_parse(), which has the following API.

> yajl\_status  
> yajl\_parse(yajl\_handle hand, const unsigned char \* jsonText,  
>  size\_t jsonTextLen);

  
As can be seen, this function simply takes a previously-created**yajl\_handle**, followed by the block of text to be parsed and its length. Calling **yajl\_parse()** on a block of text will result in the user-specified callbacks defined earlier to be called as each JSON element is encountered. To illustrate how the YAJL callback API is used in action, consider the following JSON block.

> {  
> "key1": 12345,  
> "key2": "valueString",  
> "key3": { "innerkey": 67890 }  
> }

  
When parsing the above block, the following sequence of callbacks will be invoked:  
  
yajl\_start\_map() - called when parsing the initial "{" token  
yajl\_map\_key() - called when parsing "key1"  
yajl\_integer() - called when parsing "12345"  
yajl\_map\_key() - called when parsing "key2"  
yajl\_string() - called when parsing "valuestring"  
yajl\_map\_key() - called when parsing "key3"  
yajl\_start\_map() - called when parsing the "{" token following "key3"  
yajl\_map\_key() - called when parsing "innerkey"  
yajl\_integer() - called when parsing "67890"  
yajl\_end\_map() - called when parsing the "}" token following "67890"  
yajl\_end\_map() - called when parsing the final "}" token

Vulnerability Details
---------------------

The vulnerability in question occurs during JSON deserialization of incoming SCIMP messages within libscimp, which is performed by the**scimpDeserializeMessageJSON()** function (defined in_src/SCimpProtocolFmtJSON.c_). The code is shown (edited for brevity).

> SCLError scimpDeserializeMessageJSON( SCimpContext \*ctx,  uint8\_t \*inData,   
> size\_t inSize, SCimpMsg \*\*msg)  
> {  
>     SCLError                       err = kSCLError\_NoErr;  
>     yajl\_gen\_status            stat = yajl\_gen\_status\_ok;  
>     yajl\_handle                   pHand = NULL;  
>     SCimpJSONcontext    \*jctx = NULL;  
>     uint8\_t                          \*jBuf   = NULL;  
>     size\_t                             jBufLen = 0;  
>     static yajl\_callbacks callbacks = {  
>         NULL,  
>         NULL,  
>         NULL,  
>         NULL,  
>         sParse\_number,  
>         sParse\_string,  
>         sParse\_start\_map,  
>         sParse\_map\_key,  
>         sParse\_end\_map,  
>         NULL,  
>         NULL  
>     };  
>     yajl\_alloc\_funcs allocFuncs = {  
>         yajlMalloc,  
>         yajlRealloc,  
>         yajlFree,  
>         (void \*) NULL  
>     };  
>     // check for cleartext  
>     if((inSize < (strlen(kSCIMPhdr) +1))  
>        || memcmp(  inData, kSCIMPhdr, strlen(kSCIMPhdr)) != 0)  
>     {  
>        ... handle cleartext messages ...  
>     }  
>     else  
>     {  
>         // process SCIMP message  
>         err =  scimp\_base64\_decode(inData, inSize, &jBuf, &jBufLen);   
>            CKERR;  
>         jctx = XMALLOC(sizeof (SCimpJSONcontext)); CKNULL(jctx);  
>         ZERO(jctx, sizeof(SCimpJSONcontext));  
>         pHand = yajl\_alloc(&callbacks, &allocFuncs, (void \*) jctx);  
>         yajl\_config(pHand, yajl\_allow\_comments, 1);  
>         stat = yajl\_parse(pHand, jBuf,  jBufLen); CKSTAT;  
>         stat = yajl\_complete\_parse(pHand); CKSTAT;  
>         if(IsntNull(jctx->msg))  
>             \*msg = jctx->msg;  
>    }  
>    ...  
> }

  
This function simply allocates and zeroes out a context structure (**jctx**, which is a **SCimpJSONContext** structure), establishes a YAJL handle, then invokes the **yajl\_parse\_function()**. The following code snippet shows the functions **sParse\_start\_map()** and **sParse\_end\_map()** - the pair of functions that are invoked whenever an opening brace ('{') or a closing brace ('}') is encountered respectively.

> static int sParse\_start\_map(void \* ctx)  
> {       
>     SCimpJSONcontext \*jctx = (SCimpJSONcontext\*) ctx;  
>     int retval = 1;  
>     if(IsntNull(jctx))  
>     {   
>         if(!jctx->isState && IsNull(jctx->msg))  
>         {  
>             jctx->msg = XMALLOC(sizeof (SCimpMsg));  
>             ZERO(jctx->msg, sizeof(SCimpMsg));  
>         }  
>         jctx->level++;  
>     }    
> done:   
>     return retval;  
> }  
>    
> static int sParse\_end\_map(void \* ctx)  
> {   
>     SCimpJSONcontext \*jctx = (SCimpJSONcontext\*) ctx;  
>     if(IsntNull(jctx) && IsntNull(jctx->msg))  
>     {  
>          jctx->level--;  
>     }  
>     return 1;  
> }

  
We can see in **sParse\_start\_map()** that the **jctx->msg** pointer will be initialized with a pointer to a **SCimpMsg** structure if it hasn't been initialized already, which has the following definition:

> typedef struct SCimpMsg  
> {           
>     SCimpMsgPtr                next;  
>     SCimpMsg\_Type           msgType;  
>     void\*                              userRef;  
>      union  
>     {       
>         SCimpMsg\_Commit     commit;  
>         SCimpMsg\_DH1           dh1;  
>         SCimpMsg\_DH2          dh2;  
>         SCimpMsg\_Confirm    confirm;  
>         SCimpMsg\_Data         data;  
>         SCimpMsg\_ClearTxt   clearTxt;  
>      };  
> }SCimpMsg;

  
This is a very simple structure used to contain a SCIMP message - the type is denoted by the **msgType** field, and the data values for the message are stored within the union, which is interpreted depending on the value of the **msgType** field. The message type structures contained within the above union are as follows.

> typedef struct{  
>     uint8\_t                        version;  
>     uint8\_t                        cipherSuite;  
>     uint8\_t                        sasMethod;  
>     uint8\_t                        Hpki\[SCIMP\_HASH\_LEN\];  
>     uint8\_t                        Hcs\[SCIMP\_MAC\_LEN\];  
> } SCimpMsg\_Commit;  
> typedef struct{  
>     uint8\_t                       \*pk;  
>     size\_t                          pkLen;  
>     uint8\_t                        Hcs\[SCIMP\_MAC\_LEN\];  
> } SCimpMsg\_DH1;  
> typedef struct{  
>     uint8\_t                       \*pk;  
>     size\_t                         pkLen;  
>     uint8\_t                       Maci\[SCIMP\_MAC\_LEN\];  
> } SCimpMsg\_DH2;  
> typedef struct{  
>     uint8\_t                       Macr\[SCIMP\_MAC\_LEN\];  
> } SCimpMsg\_Confirm;  
> typedef struct{  
>     uint16\_t                      seqNum;  
>     uint8\_t                       tag\[SCIMP\_TAG\_LEN\];  
>     uint8\_t                      \*msg;  
>     size\_t                         msgLen;  
>  } SCimpMsg\_Data;  
> typedef struct{  
>     uint8\_t                       \*msg;  
>     size\_t                         msgLen;  
> } SCimpMsg\_ClearTxt;

  
We will revisit some of these later.  
  
After initializing **jctx->msg**, **sParse\_start\_map()** then increments the**jctx->level** integer. This integer indicates the nesting level within the JSON message that is currently being examined. The**sParse\_end\_map()** function performs the corresponding decrement operation on the **jctx->level** variable, to indicate the end of the currently nested block. This nesting level is utilized by the **sParse\_map\_key()**callback also passed to YAJL, to indicate whether the key is a message type (occurs when nesting level is 1), or a variable name (occurs when nesting level is anything else):

> static int sParse\_map\_key(void \* ctx, const unsigned char \* stringVal,  
>                           size\_t stringLen)  
> {       
>     SCimpJSONcontext \*jctx = (SCimpJSONcontext\*) ctx;  
>     int valid = 0;  
>     if(IsntNull(jctx))  
>     {  
>         if(jctx->level == 1)  
>         {  
>             valid = sParseMsgType(jctx, stringVal, stringLen);  
>         }  
>         else  
>         {  
>             valid = sParseKey(jctx, stringVal, stringLen);  
>         }  
>     }    
>     return valid;  
> }

  
When parsing the top level of the JSON block (nesting level 1), the map key must be a valid message type, which is determined by**sParseMsgType()**.

> static int sParseMsgType(SCimpJSONcontext \* jctx, const unsigned char \* stringVal,  
>  size\_t stringLen)  
> {  
>     int valid = 0;  
>    if(jctx->isState)  
>     {  
>         ... not relevant ...  
>     }       
>     else  
>     {   
>         SCimpMsgPtr msg = jctx->msg;  
>         if(CMP(stringVal, kCommitStr, strlen(kCommitStr)))  
>         {  
>             msg->msgType = kSCimpMsg\_Commit;  
>             valid = 1;  
>         }  
>         else if(CMP(stringVal, kDH1Str, strlen(kDH1Str)))  
>         {  
>             msg->msgType = kSCimpMsg\_DH1;  
>             valid = 1;  
>         }  
>         else if(CMP(stringVal, kDH2Str, strlen(kDH2Str)))  
>         {  
>             msg->msgType = kSCimpMsg\_DH2;  
>             valid = 1;  
>          }  
>         else if(CMP(stringVal, kConfirmStr, strlen(kConfirmStr)))  
>         {  
>             msg->msgType = kSCimpMsg\_Confirm;  
>             valid = 1;  
>         }  
>         else if(CMP(stringVal, kDataStr, strlen(kDataStr)))  
>         {   
>             msg->msgType = kSCimpMsg\_Data;  
>             valid = 1;  
>        }  
>     }    
>     return valid;  
> }

  
Assuming a recognized message type is received, the **msgType** field of the **SCimpMsg** structure originally allocated in **sParse\_map\_start()** will be initialized with a constant denoting the type of the message. The value following the message type should be a sub-map, whose keys will be parsed by the **sParseKey()** function (since the **jctx->level** variable will be set to 2 when parsing this submap). The **sParseKey()** function is as follows.

> static int sParseKey(SCimpJSONcontext \* jctx, const unsigned char \* stringVal,  
>  size\_t stringLen)  
> {  
>     int valid = 0;  
>     if(jctx->isState)  
>     {  
>         ... not relevant ...  
>     }  
>     else  
>     {  
>         SCimpMsgPtr msg = jctx->msg;  
>         switch(msg->msgType)  
>         {  
>             case kSCimpMsg\_Commit:  
>                 if(CMP(stringVal, kVersionStr, strlen(kVersionStr)))  
>                 {  
>                     jctx->jType = SCimp\_JSON\_Type\_UINT8;  
>                     jctx->jItem = &msg->commit.version;  
>                     valid = 1;  
>                 }  
>                 else if(CMP(stringVal, kCipherSuiteStr, strlen(kCipherSuiteStr)))  
>                 {  
>                     jctx->jType = SCimp\_JSON\_Type\_UINT8;  
>                     jctx->jItem = &msg->commit.cipherSuite;  
>                     valid = 1;  
>                 }  
>                 else if(CMP(stringVal, kSASmethodStr, strlen(kSASmethodStr)))  
>                 {  
>                     jctx->jType = SCimp\_JSON\_Type\_UINT8;  
>                     jctx->jItem = &msg->commit.sasMethod;  
>                     valid = 1;  
>                 }  
>                 else if(CMP(stringVal, kHpkiStr, strlen(kHpkiStr)))  
>                 {  
>                     jctx->jType = SCimp\_JSON\_Type\_HASH;  
>                     jctx->jItem = &msg->commit.Hpki;  
>                     valid = 1;  
>                 }  
>                 else if(CMP(stringVal, kHcsStr, strlen(kHcsStr)))  
>                 {  
>                     jctx->jType = SCimp\_JSON\_Type\_MAC;  
>                     jctx->jItem = &msg->commit.Hcs;  
>                     valid = 1;  
>                 }  
>                 break;  
>             case kSCimpMsg\_DH1:  
>                 if(CMP(stringVal, kPKrStr, strlen(kPKrStr)))  
>                 {  
>                     jctx->jType = SCimp\_JSON\_Type\_STRING;  
>                     jctx->jItem = &msg->dh1.pk;  
>                     jctx->jItemSize = &msg->dh1.pkLen;  
>                     valid = 1;  
>                 }  
>                 else if(CMP(stringVal, kHcsStr, strlen(kHcsStr)))  
>                 {  
>                     jctx->jType = SCimp\_JSON\_Type\_MAC;  
>                     jctx->jItem = &msg->dh1.Hcs;  
>                     valid = 1;  
>                 }  
>                 break;  
>            ... more code ...  
>        }  
>    }  
>    return valid;  
> }

  
As can be seen, depending on the message type, **jctx->jType** and **jCtx->jItem** will point to the relevant field within the **SCimpMsg** structure to be filled out, along with information about what type of data the given field should be. These fields are then filled out by the **sParse\_string()** and **sParse\_number()** callbacks passed to YAJL, which are shown.

> static int sParse\_string(void \* ctx, const unsigned char \* stringVal,  
>                          size\_t stringLen)  
> {       
>     SCimpJSONcontext \*jctx = (SCimpJSONcontext\*) ctx;  
>     int valid = 0;  
>     if(jctx->jType == SCimp\_JSON\_Type\_HASH  
>        || jctx->jType == SCimp\_JSON\_Type\_MAC  
>        || jctx->jType ==  SCimp\_JSON\_Type\_TAG  
>        || jctx->jType ==  SCimp\_JSON\_Type\_STATE\_TAG)  
>     {   
>         uint8\_t     buf\[ MAX( MAX( SCIMP\_HASH\_LEN, SCIMP\_MAC\_LEN) , SCIMP\_TAG\_LEN)  \];  
>         size\_t      dataLen = sizeof(buf);  
>       if( base64\_decode(stringVal,  stringLen, buf, &dataLen) == CRYPT\_OK)  
>       {  
>          if((jctx->jType == SCimp\_JSON\_Type\_HASH&& dataLen == SCIMP\_HASH\_LEN)  
>          || (jctx->jType == SCimp\_JSON\_Type\_MAC&& dataLen == SCIMP\_MAC\_LEN)  
>          || (jctx->jType == SCimp\_JSON\_Type\_TAG&& dataLen == SCIMP\_TAG\_LEN)  
>          || (jctx->jType == SCimp\_JSON\_Type\_STATE\_TAG&& dataLen ==   
> SCIMP\_STATE\_TAG\_LEN))  
>          {  
>              COPY(buf, jctx->jItem, dataLen);  
>              valid = 1;  
>          }  
>       }  
>      }  
>     else if(jctx->jType == SCimp\_JSON\_Type\_STRING)  
>     {  
>         size\_t      dataLen =  stringLen ;  
>         uint8\_t     \*buf =  XMALLOC(stringLen);  
>         if( base64\_decode(stringVal,  stringLen, buf, &dataLen) == CRYPT\_OK)  
>         {  
>             \*((size\_t\*) jctx->jItemSize) = dataLen;  
>             \*((uint8\_t\*\*) jctx->jItem) = buf;  
>             valid = 1;  
>         }  
>         else  
>         {  
>             XFREE(buf);  
>         }  
>     }  
>     return valid;  
> }  
> static int sParse\_number(void \* ctx, const char \* str, size\_t len)  
> {  
>     SCimpJSONcontext \*jctx = (SCimpJSONcontext\*) ctx;  
>     char buf\[32\] = {0};  
>     int valid = 0;  
>     if(len < sizeof(buf))  
>     {  
>         COPY(str,buf,len);  
>         if(jctx->jType == SCimp\_JSON\_Type\_UINT8)  
>         {  
>             uint8\_t val = atoi(buf);  
>             \*((uint8\_t\*) jctx->jItem) = val;  
>             valid = 1;  
>         }  
>         if(jctx->jType == SCimp\_JSON\_Type\_UINT16)  
>         {  
>             uint16\_t val = atoi(buf);  
>             \*((uint16\_t\*) jctx->jItem) = val;  
>             valid = 1;  
>         }  
>     }  
>     return valid;  
> }

  
The problem with the way SCIMP employs the YAJL library is that the **sParse\_map\_key()** function will call the **sParseMsgType()** function potentially more than once while parsing a single JSON block, which will have the result of altering the message type even after fields in the SCimpMsg union have been filled out by **sParse\_string()**/**sParse\_number()**. For example, consider what happens when the following message is received:

> {  
> "dh2":  
> {  
>   "PKi": "MGwDAgcAAgEwAjEAkzRmb7bD/ihDZ7e0zpHvOnDf  
>    JcJKU1vlKjJJlTegFt7Kxps4kdiBQdaOd8ALOH3jAj  
>    Brm/r6BInlydGJASGuAGGg7QPJTWlVz2SwKWRCkNgu/FTEuTLycQN44slH3iVAfYs=",  
>   "maci": "+SlATBJtOG8="  
> },  
> "data":  
> {  
>   "seq": 32896  
> },  
> "dh2": null  
> }

  
This message will be parsed by the YAJL library, resulting in the following callbacks:  
  
sParse\_start\_map() - Allocates jctx->msg, zeroes it, and sets jctx->level to 1  
sParse\_map\_key() - indicates that this is a "dh2" message  
sParse\_start\_map() - Increments jctx->level to 2  
sParse\_map\_key() - Parse and set pk and pkLen fields in the SCimpMsg structure  
sParse\_map\_key() - Parse the "maci" field and set Maci\[\] array  
sParse\_end\_map() - Decrements jctx->level to 1  
sParse\_map\_key() - indicates this is a "data" message \*\* OVERWRITES msg->msgType field with a new type \*\*\*  
sParse\_start\_map() - Increments jctx->field to 2  
sParse\_map\_key() - Parses the seq field, which is a 16-bit integer. This field overlaps with the low 2 bytes of the "pk" pointer allocated previously  
sParse\_end\_map() - Decrements jctx->level to 1  
sParse\_map\_key() - Indicates this is a "dh2" message, again overwriting msg->msgType field. Now we have a dh2 message, with the pk pointer modified with arbitrary contents by the attacker.  
  
  
By resetting the **jctx->msg->msgType** field with the "dh2" attribute at the end of the message, a type confusion vulnerability will occur where the seq fields supplied in the "data" message will be incorrectly interpreted as the pk field - a raw memory pointer. (In this case, the low two bytes have been set to 0x8080.) Note that by utilizing messages other than "data", we could arbitrarily modify the entire pointer (and the pkLen field, indicating how much data pk points to). Assuming that we are at the correct phase of protocol negotiation, sending this message results in the following crash:  
  
\]Fatal signal 11 (SIGSEGV) at 0xdeadbaad (code=1), thread 17201 (com.silentcircl)  
I/DEBUG   ( 9735): \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\* \*\*\*  
I/DEBUG   ( 9735): Revision: '0'  
I/DEBUG   ( 9735): pid: 15611, tid: 17201, name: com.silentcircl  >>> com.silentcircle.silenttext <<<  
I/DEBUG   ( 9735): signal 11 (SIGSEGV), code 1 (SEGV\_MAPERR), fault addr deadbaad  
**I/DEBUG   ( 9735): Abort message: 'invalid address or address of corrupt block 0x601b8078 passed to dlfree'**   
I/DEBUG   ( 9735):     r0 00000000  r1 400d757e  r2 deadbaad  r3 400db0cd  
I/DEBUG   ( 9735):     r4 601b8078  r5 400e5180  r6 40086000  r7 601b8080  
I/DEBUG   ( 9735):     r8 2910001d  r9 60a11110  sl 0000000e  fp 618969b8  
I/DEBUG   ( 9735):     ip 00000001  sp 62d67930  lr 400a87e3  pc 400a87e4  cpsr 60070030  
I/DEBUG   ( 9735):     d0  2064657372666c64  d1  2073736572646461  
I/DEBUG   ( 9735):     d2  657264646120726f  d3  6f6320666f207373  
I/DEBUG   ( 9735):     d4  0000000000000000  d5  0000000000000000  
I/DEBUG   ( 9735):     d6  0000000000000000  d7  c1436f2400000000  
I/DEBUG   ( 9735):     d8  0000000000000000  d9  0000000000000000  
I/DEBUG   ( 9735):     d10 0000000000000000  d11 0000000000000000  
I/DEBUG   ( 9735):     d12 0000000000000000  d13 0000000000000000  
I/DEBUG   ( 9735):     d14 0000000000000000  d15 0000000000000000  
I/DEBUG   ( 9735):     d16 41d5226dd0000000  d17 41d518b63e000000  
I/DEBUG   ( 9735):     d18 c1436f2400000000  d19 0000000000000000  
I/DEBUG   ( 9735):     d20 0000002f0000002f  d21 0000002f0000002f  
I/DEBUG   ( 9735):     d22 0000000000000000  d23 0000000000000000  
I/DEBUG   ( 9735):     d24 0003000000030000  d25 0003000000030000  
I/DEBUG   ( 9735):     d26 0707070703030303  d27 000000300000002f  
I/DEBUG   ( 9735):     d28 0001000000010000  d29 0001000000010000  
I/DEBUG   ( 9735):     d30 00b6800000b38000  d31 00bc800000b98000  
I/DEBUG   ( 9735):     scr 80000010  
I/DEBUG   ( 9735):  
I/DEBUG   ( 9735): backtrace:  
I/DEBUG   ( 9735):     #00  pc 000117e4  /system/lib/libc.so (dlfree+1191)  
I/DEBUG   ( 9735):     #01  pc 0000dd23  /system/lib/libc.so (free+10)  
I/DEBUG   ( 9735):     #02  pc 00012678  /data/app-lib/  
com.silentcircle.silenttext-1/libscimp.so (scimpFreeMessageContent+204)  
I/DEBUG   ( 9735):     #03  pc 000137a0  /data/app-lib/  
com.silentcircle.silenttext-1/libscimp.so  
I/DEBUG   ( 9735):     #04  pc 00012440  /data/app-lib/  
com.silentcircle.silenttext-1/libscimp.so (sProcessTransition+96)  
I/DEBUG   ( 9735):     #05  pc 000124c8  /data/app-lib/  
com.silentcircle.silenttext-1/libscimp.so (scTriggerSCimpTransition+52)  
I/DEBUG   ( 9735):     #06  pc 000147fc  /data/app-lib/  
com.silentcircle.silenttext-1/libscimp.so (SCimpProcessPacket+308)  
I/DEBUG   ( 9735):     #07  pc 00002ff0  /data/app-lib/  
com.silentcircle.silenttext-1/libscimp-jni.so (SCimpPacket\_receivePacket+28)  
I/DEBUG   ( 9735):     #08  pc 0000470c  /data/app-lib/  
com.silentcircle.silenttext-1/libscimp-jni.so (Java\_com\_silentcircle\_scimp\_NativePacket\_receivePacketPKI+1468)  
  
  
The highlighted line shows the invalid address  0x601b8080 passed to free(), which is the pointer we corrupted. By manipulating the mechanics of the underlying heap or of application data structures themselves, it is possible to leverage this flaw to gain arbitrary code execution.

#### [Source](https://www.azimuthsecurity.com/blog/2015/11/16/blackpwn-blackphone-silenttext-type-confusion-vulnerability)

<br/>
---
