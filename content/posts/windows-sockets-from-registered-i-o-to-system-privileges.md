---
title: "Windows Sockets: From Registered I/O to SYSTEM Privileges"
date: 2025-01-15
tags: 
  - "ces"
  - "edge"
  - "exploit-techniques"
  - "vulnerability-analysis"
  - "zero-day"
---

By Luca Ginex

## Overview

This post discusses CVE-2024-38193, a use-after-free vulnerability in the afd.sys Windows driver. Specifically, the vulnerability is in the Registered I/O extension for Windows sockets. The vulnerability was patched in the August 2024 Patch Tuesday. This post describes the exploitation process for the vulnerability.

First, we give a general overview of the registered I/O extension for Winsock, describing the driver’s internal structures. We then analyze the vulnerability and proceed to detailing the exploitation strategy.

## Preliminaries

In this section we give a general overview of the registered I/O extension for Winsock and describe the relevant structures for registering I/O extensions.

### Winsock Registered I/O Extension

In Windows, the Registered I/O (RIO) extension can be used in socket programming in order to reduce the amount of system calls issued by userland programs while sending and receiving packets. The RIO extension workflow is as follows:

- The userland program registers huge buffers. The kernel then obtains kernel mappings for them.
- The userland program issues send and receive requests by using the receive and send buffer _slices_ of the registered buffers.

If a userland program wants to use the Registered I/O extension for sockets, it has to create a socket via the `WSASocketA()` function.  
The function’s prototype is as follows:

				
					`SOCKET WSAAPI WSASocketA(   [in] int                 af,   [in] int                 type,   [in] int                 protocol,   [in] LPWSAPROTOCOL_INFOA lpProtocolInfo,   [in] GROUP               g,   [in] DWORD               dwFlags );`
				
			

The userland program has to specify the `WSA_FLAG_REGISTERED_IO` flag in the `dwFlags` argument of the call. The program then has to retrieve the function table for the RIO API. This table can be retrieved by issuing a `WSAIoctl()` call with the `SIO_GET_MULTIPLE_EXTENSION_FUNCTION_POINTER` IOCTL code. The call returns a `RIO_EXTENSION_FUNCTION_TABLE` structure. This table contains all the pointers to the RIO API. The definition of the structure is presented in the following code listing:

				
					`typedef struct _RIO_EXTENSION_FUNCTION_TABLE {   DWORD                         cbSize;   LPFN_RIORECEIVE               RIOReceive;   LPFN_RIORECEIVEEX             RIOReceiveEx;   LPFN_RIOSEND                  RIOSend;   LPFN_RIOSENDEX                RIOSendEx;   LPFN_RIOCLOSECOMPLETIONQUEUE  RIOCloseCompletionQueue;   LPFN_RIOCREATECOMPLETIONQUEUE RIOCreateCompletionQueue;   LPFN_RIOCREATEREQUESTQUEUE    RIOCreateRequestQueue;   LPFN_RIODEQUEUECOMPLETION     RIODequeueCompletion;   LPFN_RIODEREGISTERBUFFER      RIODeregisterBuffer;   LPFN_RIONOTIFY                RIONotify;   LPFN_RIOREGISTERBUFFER        RIORegisterBuffer;   LPFN_RIORESIZECOMPLETIONQUEUE RIOResizeCompletionQueue;   LPFN_RIORESIZEREQUESTQUEUE    RIOResizeRequestQueue; } RIO_EXTENSION_FUNCTION_TABLE, *PRIO_EXTENSION_FUNCTION_TABLE;`
				
			

Once the function table is obtained, the program has to register the I/O buffers. These buffers will be used for all subsequent I/O operations. In order to do so, the `RIORegisterBuffer()` function must be called.

The function’s prototype is as follows:

				
					`RIO_BUFFERID  RIORegisterBuffer(   _In_ PCHAR DataBuffer,   _In_ DWORD DataLength );`
				
			

The \`DataBuffer\` argument is a pointer to the buffer to be used and the \`DataLength\` argument is the size of the buffer. The function returns a \`RIO\_BUFFERID\` buffer descriptor, which is an opaque integer that identifies the registered buffer in the kernel.

In order to send or receive data from the socket, the \`RIOSend()\` and the \`RIOReceive()\` functions can be used. These functions accept a \`RIO\_BUF\` structure that describes a \*slice\* of the registered buffers to be used. The definition of the \`RIO\_BUF\` structure is as follows:

				
					`typedef struct _RIO_BUF {   RIO_BUFFERID BufferId;   ULONG        Offset;   ULONG        Length; } RIO_BUF, *PRIO_BUF;`
				
			

### Kernel Structures

Structure definitions are obtained by reverse engineering and may not accurately reflect structures defined in the source code. The recovered structure definition for the registered buffer is as follows:

				
					`struct RIOBuffer {   PMDL AssociatedMDL;   _QWORD VirtualAddressBuffer;   _DWORD LengthBuffer;   _DWORD RefCount;   _DWORD IsInvalid;   _DWORD Unknown; };`
				
			

This kernel structure is used by the `afd.sys` driver to keep track of the registered buffer. It contains the following fields:

- `AssociatedMDL`: A pointer to the MDL that describes the userland buffer.
- `VirtualAddressBuffer`: A virtual kernel pointer that can be used by the kernel driver to write/read to the userland buffer.
- `LengthBuffer`: The size of the userland buffer.
- `RefCount`: Field that keeps track of the number of references to this buffer.
- `IsInvalid`: Field that encodes the state of the buffer. `0` means that it is in use, `1` means that it is freed and `2` means that it has to be  
    freed.
- `Unknown`: Reserved.

The `afd.sys` driver keeps all the `RIOBuffer` structures in an array, one entry for each registered buffer. The `afd.sys` driver also creates, for efficiency reasons, a _cache_ for the most recently used `RIOBuffer` elements of the array. Each entry in the cache has the following structure:

				
					`struct CachedBuffer {   _DWORD IdBuffer;   _DWORD RefCount;   RIOBuffer *BufferPtr; };`
				
			

It contains the following fields:

- `IdBuffer`: The integer identifier associated with the registered buffer.
- `RefCount`: Reference counter for the cached entry.
- `BufferPtr`: Pointer to the `RIOBuffer` structure this entry is mirroring.

The following image is a visual representation of the afd.sys RIO component.

<figure>

![](https://blog.exodusintel.com/wp-content/uploads/2024/12/img1.png)

<figcaption>

afd.sys RIO component

</figcaption>

</figure>

## Vulnerability

The use-after-free vulnerability is caused by a race condition between the `AfdRioGetAndCacheBuffer()` and `AfdRioDereferenceBuffer()` functions. The `AfdRioGetAndCacheBuffer()` function needs to access the registered buffers involved in send/receive operations, therefore it temporarily increments the reference counter of the involved registered buffers by using the `_InterlockedIncrement()` intrinsic function.

The `AfdRioDereferenceBuffer()` function is called when a usermode application calls the `RIODeregister()` API function. This function checks the reference counter’s value associated with the registered buffer that the usermode application wants to deregister. If the reference counter’s value is one, then the function proceeds to free the structure. The race condition between these functions, allows a malicious user to force the `AfdRioGetAndCacheBuffer()` function to act on a registered buffer structure that has been freed by the `AfdRioDereferenceBuffer()` function.

### IO Buffer Registration

When a user-mode application registers an I/O buffer using `RIORegisterBuffer()`, this ultimately calls the `AfdRioCreateRegisteredBuffer()`  
function in the afd.sys driver.

				
					`// Module: afd.sys <div></div> __int64 __fastcall AfdRioCreateRegisteredBuffer(         struct_FsContext *FsContext,         _MDL *a2,         __int64 VirtualAddr,         int Len,         unsigned int *a5,         PMDL **a6) {   RioBuffer = 0i64;   memset(＆LockHandle, 0, sizeof(LockHandle));   v8 = 1;   v9 = 1;   *a6 = 0i64;   AfdAcquireWriteLock(FsContext-＞SpinLock, ＆LockHandle);   if ( FsContext-＞byte88 )   {     v10 = STATUS_INVALID_DEVICE_STATE;     goto LABEL_17;   }   LastIdx = FsContext-＞LastIdx; <div></div> [1] <div></div>   ArrayBuffers = ＆FsContext-＞ArrayBuffers;   v13 = ＆FsContext-＞NumBuffers; <div></div> [2] <div></div>   while ( 1 )   {     if ( !LastIdx )       goto LABEL_7;     RioBuffer = (RIOBuffer *)(*ArrayBuffers)[LastIdx];     if ( !RioBuffer )       break;     if ( RioBuffer-＞IsInvalid == 2 )     {       v8 = 0;       goto LABEL_13;     } LABEL_7:     v14 = LastIdx;     v15 = LastIdx + 1;     LastIdx = 0;     if ( v14 != *v13 - 1 )       LastIdx = v15;     if ( LastIdx == FsContext-＞LastIdx )       goto LABEL_14;   }   v9 = 0; LABEL_13:   FsContext-＞LastIdx = LastIdx; LABEL_14:   if ( v8 )   { <div></div> [Truncated] <div></div> [3] <div></div>     RioBuffer = (RIOBuffer *)ExAllocatePool2(97i64, 0x20i64, 'bOIR');     (*ArrayBuffers)[LastIdx] = (__int64)RioBuffer;   }   RioBuffer-＞AssociatedMDL = a2;   RioBuffer-＞VirtualAddressBuffer = VirtualAddr;   RioBuffer-＞LengthBuffer = Len;   RioBuffer-＞RefCount = 1;   RioBuffer-＞IsInvalid = 0;   *a5 = LastIdx;   *a6 = ＆RioBuffer-＞AssociatedMDL;   KeReleaseInStackQueuedSpinLock(＆LockHandle);   return 0i64; }`
				
			

At \[1\], the `AfdRioCreateRegisteredBuffer()` function retrieves the array of registered buffers and at \[2\], in the `while` loop, it finds the next free index ID to assign to the new buffer. Once found, at \[3\], memory for the new `RIOBuffer` structure is allocated and its `RefCount` is initialized to 1.

### IO Buffer Usage

Whenever the `afd.sys` kernel driver needs to look up the array buffer (when sending or receiving packets, for example) it temporarily increments the reference count of that specific `RIOBuffer` structure and it stores it in the cache. An example of this behavior can be seen in the `AfdRioValidateRequestBuffer()` function below.

				
					`// Module: afd.sys <div></div> char __fastcall AfdRioValidateRequestBuffer(         _QWORD *a1,         struct _KLOCK_QUEUE_HANDLE *QueueLock,         _BYTE *ReadLockAcquired,         unsigned int *a4,         RIOBuffer **a5) {   NumBuffer = *a4;   if ( NumBuffer )   { <div></div> [4] <div></div>     CachedBuffer = (RIOBuffer *)AfdRioGetCachedBuffer((__int64)a1, QueueLock, ReadLockAcquired, NumBuffer);     if ( CachedBuffer )     {       Offset = a4[1];       Length = a4[2];       if ( Length + Offset ＞= Offset ＆＆ Length + Offset ＜= CachedBuffer-＞LengthBuffer )       {         *a5 = CachedBuffer;         return 1;       }       AFDETW_RIO_TRACE_INVALID_BUFFER_RANGE(*a1, (int)a1, (int)CachedBuffer, Offset, Length);       AfdRioDereferenceCachedBuffer((__int64)a1, *a4, CachedBuffer);       result = 0;     }     else     {       AFDETW_RIO_TRACE_INVALID_BUFFERID(*a1, a1, *a4);       result = 0;     }   }   else   {     result = 1;   }   *a5 = 0i64;   return result; }`
				
			

This function is called when a userland program uses the `RIOSend()` function API. The `AfdRioValidateRequestBuffer()` function is in charge of validating the `RIO_BUF`structure passed as an argument to the function API. At \[4\], the code calls the `AfdRioGetCachedBuffer()` function passing the identifier for the registered buffer. This function returns the `CachedBuffer` structure corresponding to that specific identifier if the ID is valid, otherwise it returns zero.

The `AfdRioGetCachedBuffer()` function is shown below.

				
					`// Module: afd.sys <div></div> RIOBuffer *__fastcall AfdRioGetCachedBuffer(         struct_a1 *a1,         struct _KLOCK_QUEUE_HANDLE *a2,         _BYTE *a3,         unsigned int NumBuffer) {   CachedBufferArrays = a1-＞CachedBufferArrays;   v8 = NumBuffer % a1-＞ModuloCachedBuffers;   BufferPtr = CachedBufferArrays[v8].BufferPtr; <div></div> [5] <div></div>   if ( BufferPtr ＆＆ !BufferPtr-＞IsInvalid ＆＆ CachedBufferArrays[v8].IdBuffer == NumBuffer )   {     v10 = CachedBufferArrays[v8].RefCount++ == -1;     CachedBufferArrays_1 = a1-＞CachedBufferArrays;     if ( v10 )     {       --CachedBufferArrays_1[v8].RefCount;       return 0i64;     }     else     { <div></div> [6] <div></div>       return CachedBufferArrays_1[v8].BufferPtr;     }   }   else   {     if ( !*a3 )     {       AfdAcquireReadLockAtDpcLevel(a1-＞FsContext-＞SpinLock, a2);       *a3 = 1;     } <div></div> [7] <div></div>     return AfdRioGetAndCacheBuffer(a1, NumBuffer);   } }`
				
			

The `AfdRioGetCachedBuffer()` function checks if the requested buffer is already cached \[5\]. If so, the functions returns the structure without further processing \[6\]. If the requested buffer is not in the cache, the `AfdRioGetCachedBuffer()` function has to evict one of the entries in the cache to store the requested buffer. This is done in the `AfdRioGetAndCacheBuffer()` function \[7\] shown below.

				
					`// Module: afd.sys <div></div> RIOBuffer *__fastcall AfdRioGetAndCacheBuffer(struct_a1 *a1, unsigned int NumBuffer) {   v2 = a1-＞FsContext;   v4 = ＆a1-＞CachedBufferArrays[NumBuffer % a1-＞ModuloCachedBuffers];   if ( a1-＞FsContext-＞byte88 )     return 0i64;   if ( NumBuffer ＞= v2-＞NumBuffers )     return 0i64;   _mm_lfence();   v5 = (RIOBuffer *)v2-＞ArrayBuffers[NumBuffer];   if ( !v5 || v5-＞IsInvalid )     return 0i64; <div></div> [8] <div></div>   _InterlockedIncrement(＆v5-＞RefCount);   if ( AfdRioEvictCachedBuffer(v4) )   {     v4-＞BufferPtr = v5;     v4-＞IdBuffer = NumBuffer;     v4-＞RefCount = 1;   }   return v5; }`
				
			

At \[8\], the `AfdRioGetCachedBuffer()` function temporarily increments the reference counter of the `RIOBuffer` structure associated with the specific identifier by calling the `_InterlockedIncrement()` intrinsic function. The usage of this intrinsic only guarantees an atomic increment of the variable. Then the `AfdRioGetAndCacheBuffer()` function calls the `AfdRioEvictCachedBuffer()` function to evict the cache entry. If the operation is successful, the cache entry is populated with the new buffer data.

### IO Buffer Deregistration

When a userland program wants to deregister an RIO buffer it can do so by calling the `RIODeregisterBuffer()` function API which subsequently invokes the `AfdRioDereferenceBuffer()` function in the `afd.sys` kernel driver.

The `AfdRioDereferenceBuffer()` function is shown below.

				
					`// Module: afd.sys <div></div> void __fastcall AfdRioDereferenceBuffer(struct_FsContext *a1, RIOBuffer *a2, unsigned int NumBuffer) {   v4 = NumBuffer; <div></div> [9] <div></div>   if ( a2-＞RefCount == 1 || _InterlockedExchangeAdd(＆a2-＞RefCount, -1u) == 1 )   {     SpinLock = a1-＞SpinLock;     memset(＆LockHandle, 0, sizeof(LockHandle));     AfdAcquireWriteLock(SpinLock, ＆LockHandle);     a1-＞ArrayBuffers[v4] = 0i64;     KeReleaseInStackQueuedSpinLock(＆LockHandle);     AfdRioCleanupBuffer(a2, 1);   } }`
				
			

At \[9\], the `AfdRioDereferenceBuffer()` function checks if the reference counter for that specific `RIOBuffer` structure is set to `1`. If so, the `RIOBuffer` structure is freed in the `AfdRioCleanupBuffer()` function.

A race condition exists here that allows a malicious user to schedule the execution of the `AfdRioGetAndCacheBuffer()` and the `AfdRioDereferenceBuffer()` functions in different CPUs at the same time acting on the same registered buffer. If the malicious user is able to win the race condition, at \[8\], the new cached buffer contains a `BufferPtr` field pointing to a memory region freed by the thread executing the `AfdRioDereferenceBuffer()` function on the same registered buffer resulting in a use-after-free vulnerability.

## Exploitation

Exploiting this vulnerability involves the following steps:

1. Heap Spraying Stage:
    
    - Spraying the non-paged pool with fake `RIOBuffer` structures.
    - Creating holes in the non-paged pool.
    - Registering I/O buffers. 
2. Triggering the use-after-free vulnerability on one of the previously-allocated RIOBuffer structure
    
3. Privilege Escalation
    

### Heap Spraying Stage

Since the vulnerable buffer is allocated in the non-paged pool, the spray technique we used leverages _named pipes_ to fill the non-paged pool area with arbitrary-sized buffers. Readers unfamiliar with named pipes heap spraying techniques can read more about them here and here. The target size is the same one as the `RIOBuffer`‘s size, which is `0x20` bytes. There are a lot of documented techniques that use named pipes for heap spraying. In this case we used _unbuffered entries_. The advantage over _buffered entries_ is that unbuffered entries don’t have an header. This makes them ideal to perfectly match `RIOBuffer`‘s size. After the spray, the non-paged pool layout is as follows:

<figure>

![](https://blog.exodusintel.com/wp-content/uploads/2024/12/img2.png)

<figcaption>

Non-page pool after being sprayed

</figcaption>

</figure>

It’s important to note that the content of the unbuffered entries is fully controlled by the exploit. The next step is to create holes inside the non-paged pool by closing some of the previously-allocated named pipes. This leaves room for `RIOBuffer` structures to be allocated next to unbuffered entries. In order to fill the holes, the exploit must call the `RegisterBuffer()` function.

<figure>

![](https://blog.exodusintel.com/wp-content/uploads/2024/12/img3-1024x606.png)

<figcaption>

Creating holes in the non-paged pool

</figcaption>

</figure>

### Triggering the vulnerability

With the non-paged pool setup completed, we can trigger the use-after-free vulnerability. In order to trigger it, the exploit must create two concurrent threads: one that keeps using the registered buffers by issuing read/write requests and the other one that loops through all the registered buffers and tries to deregister them. If the race condition is successful, the afd.sys will have entries in `CachedArray` that are pointing to freed `RIOBuffer` structures.

<figure>

![](https://blog.exodusintel.com/wp-content/uploads/2024/12/img4-1024x714.png)

<figcaption>

Triggering the use-after-free

</figcaption>

</figure>

### Privilege Escalation

Once the use-after-free vulnerability has been triggered, some of the `RIOBuffer` structures are freed but remain in use in the afd.sys cache. The exploit must re-allocate them in order to gain control over the contents of the `RIOBuffer` structures. The exploit can again use unbuffered entries. If the exploit operations are successful, the exploit now controls the content of some `RIOBuffer` structures that are still alive in the cache.

<figure>

![](https://blog.exodusintel.com/wp-content/uploads/2024/12/img5-1024x714.png)

<figcaption>

Exploit using some unbuffered entries

</figcaption>

</figure>

In order to create arbitrary read and arbitrary write primitives, the exploit leverages the internal mechanism of the `RIOSend()` and `RIOReceive()` functions. These two functions use the `AssociatedMDL` field of the `RIOBuffer` structure to copy data to/from the registered buffer. Since the exploit has control over the `AssociatedMDL`field, it can craft an arbitrary MDL in userland having the `MappedSystemVa` field pointing to an arbitrary address.

By issuing a `RIOSend()` call, data is copied from the `MappedSystemVa` address to the packet that is sent to the network. Vice versa, by issuing a `RIOReceive()` call, data is copied from the packet coming from the network to the `MappedSystemVa` address. In our case, the arbitrary write primitive is very interesting. A good candidate kernel address for the arbitrary write primitive is the address of the `_SEP_TOKEN_PRIVILEGES`structure of the exploit process. By overwriting the `_SEP_TOKEN_PRIVILEGES` structure it is possible to escalate to `nt authoritysystem` privileges.

## Conclusion

In this blogpost we described a use-after-free vulnerability in the afd.sys Windows driver patched in the August 2024 patch Tuesday. We also presented a possible exploitation technique to achieve privilege escalation to `nt authoritysystem` privileges. We tested the exploit on a Windows 11 21H2 machine and the heap spraying routine proved to be very stable

## About Exodus Intelligence

Our world class team of vulnerability researchers discover hundreds of exclusive Zero-Day vulnerabilities, providing our clients with proprietary knowledge before the adversaries find them. We also conduct N-Day research, where we select critical N-Day vulnerabilities and complete research to prove whether these vulnerabilities are truly exploitable in the wild.

For more information on our products and how we can help your vulnerability efforts, visit www.exodusintel.com or contact info@exodusintel.com for further discussion.

The post Windows Sockets: From Registered I/O to SYSTEM Privileges appeared first on Exodus Intelligence.

Go to Source
