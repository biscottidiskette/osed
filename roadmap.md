# OSED Prep Roadmap

The phases below are aligned to the EXP-301 module order and the core exploit development skill areas. Each phase builds on the last. **Do not skip ahead** — this material rewards depth, not breadth.

---

## Phase 0: Foundation & Setup (1–2 weeks)

**Goal: Lock in C fundamentals, x86 assembly, Windows internals, PE format, and tool fluency before touching exploits.**

### Topics:
- C programming: pointers, arrays, strings, structs, memory allocation (malloc/free)
- Understanding what vulnerable C code looks like before the compiler turns it into assembly
- x86 Assembly (32-bit focus): registers, stack operations, calling conventions (stdcall/cdecl)
- Stack layout: function prologs/epilogs, EBP/ESP relationship
- PE file format: headers, sections, IAT, image loading
- Virtual memory, Windows API basics (VirtualAlloc, LoadLibrary, GetProcAddress)
- WinDbg workflow: breakpoints, stepping, memory inspection, !address, !exchain
- IDA workflow: navigation, cross-references, string analysis, function identification

### Tools:
- WinDbg Preview
- IDA Freeware
- PE-bear

### Resources:
- *The C Programming Language* — Kernighan and Ritchie (or *C Programming: A Modern Approach* — K.N. King)
- [Learn-C](https://www.learn-c.org/)
- [OpenSecurityTraining — Intro to x86](https://opensecuritytraining.info/IntroX86.html)
- *Practical Reverse Engineering* — Bruce Dang (Ch. 1–3)
- *Windows Internals, Part 1* — Mark Russinovich (Ch. 1–4)
- *The IDA Pro Book* — Chris Eagle (Ch. 1–3)
- [Microsoft — WinDbg Preview Guide](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools)
- [Mark Pelf — PE Format Illustrated](https://markpelf.com/1628/pe-format-illustrated-part-1/)
- [Microsoft — PE Format Specification](https://learn.microsoft.com/en-us/windows/win32/debug/pe-format)

---

## Phase 1: Stack Overflows & SEH (2–3 weeks)

**Goal: Master vanilla buffer overflows and SEH-based exploitation — the foundation for everything that follows.**

### Topics:
- Stack-based buffer overflows: manual EIP control
- Cyclic pattern generation and offset calculation
- Bad character identification and handling
- SEH overwrite technique and nSEH/SEH chain mechanics
- Short jumps and trampolining

### Tools:
- WinDbg: breakpoints, memory inspection, !exploitable, !exchain
- IDA: string analysis, function mapping
- Python 3: pattern generation and automation scripts

### Tasks:
- Reproduce vanilla buffer overflow (gain EIP control)
- Manually find offsets using cyclic patterns
- Identify bad characters using memory dump comparison
- Trigger and redirect execution via SEH overwrite
- Use POP-POP-RET to redirect to nSEH

### Resources:
- [Corelan Exploit Writing Parts 1–3 (Stack, SEH)](https://www.corelan.be/index.php/articles/)
- *The Shellcoder's Handbook* — Jack Koziol (buffer overflow chapters)
- [FuzzySecurity — Exploit Dev Tutorials](https://www.fuzzysecurity.com/tutorials.html)
- [LiveOverflow — Binary Exploitation](https://www.youtube.com/playlist?list=PLhixgUqwRTjxglIswKp9mpkfPNfHkzyeN)

---

## Phase 2: Egghunters (1–2 weeks)

**Goal: Overcome space restrictions by using egghunters to locate shellcode in memory.**

### Topics:
- Why egghunters: limited buffer space after gaining EIP control
- NtAccessCheckAndAuditAlarm and NtDisplayString syscall egghunters
- Egg tag placement strategies
- Combining egghunters with SEH-based delivery

### Tasks:
- Write an egghunter from scratch using NtAccessCheckAndAuditAlarm syscall
- Deploy egghunter + staged shellcode against a SEH overflow with limited buffer
- Test with different egg tags and verify memory scanning in WinDbg

### Resources:
- [Corelan — Writing Egghunters](https://www.corelan.be/index.php/2009/09/28/writing-egg-hunters-with-buffer-overflows/)
- [FuzzySecurity — Stack Smashing 0x08](https://www.fuzzysecurity.com/tutorials/expDev/8.html)
- [SkullSecurity — Egghunter Explanation](https://www.skullsecurity.org/blog/2010/writing-egg-hunters)

---

## Phase 3: IDA Reverse Engineering (2–3 weeks)

**Goal: Become fluent in static analysis — RE is how you find the bug before you can exploit it.**

### Topics:
- Navigating IDA: Functions window, Strings window, Imports, cross-references (Xrefs)
- Identifying vulnerability patterns: unsafe string operations, unchecked buffer copies
- Tracing data flow from input to vulnerable function
- Recognizing calling conventions and reconstructing function prototypes
- Patching binaries in IDA for testing
- Mapping IDA static analysis to WinDbg dynamic analysis
- Crackmes: tracing program logic in IDA with no source code to fall back on

### Tasks:
- Complete 5+ crackmes from [crackmes.one](https://crackmes.one/)
- RE a simple vulnerable binary: find the overflow in IDA, confirm in WinDbg
- RE a network-facing binary: identify the protocol parsing, find the input vector
- Practice on 3+ binaries without looking at existing writeups first

### Resources:
- *The IDA Pro Book* — Chris Eagle (full analysis sections)
- *Practical Reverse Engineering* — Bruce Dang (reversing chapters)
- [Crackmes.one](https://crackmes.one/) — practice RE challenges

---

## Phase 4: Custom Shellcode from Scratch (4–5 weeks)

**Goal: Write Windows shellcode entirely by hand in x86 assembly. This includes building multiple payloads beyond those included in class for variety and to anchor the skill.**

### Topics:
- PEB walking: resolving kernel32.dll base address at runtime
- Export table parsing: finding function addresses by hash (GetProcAddress equivalent)
- Position-independent code (PIC) — no hardcoded addresses
- Null-byte avoidance and bad character constraints
- Writing a reverse shell from scratch
- Writing a dropper/stager from scratch
- Writing an add-user-to-admin payload from scratch
- Custom encoder/decoder stubs: XOR encoding to evade bad character filters
- Alphanumeric and Unicode shellcode constraints

### Key Win32 APIs:
- **LoadLibraryA** — load a DLL at runtime
- **GetProcAddress** — resolve function addresses from loaded DLLs
- **URLDownloadToFileA** — download a file from a URL to a local path
- **WinExec / CreateProcessA** — execute a local file
- **WSAStartup / WSASocketA / connect** — Winsock networking
- **NetUserAdd / NetLocalGroupAddMembers** — local user management
- **ExitProcess** — clean exit
- Understand the **stdcall** convention: arguments pushed right-to-left, callee cleans the stack

### Tasks:
- Write a WinExec('calc.exe') shellcode from scratch
- Build a reverse TCP shell (ws2_32.dll)
- Build a dropper/stager (urlmon.dll)
- Build an add-local-admin payload (netapi32.dll)
- Write a XOR encoder in Python and decoder stub in assembly
- Write a bind shell from scratch
- Write shellcode that works under Unicode constraints

### Resources:
- *The Shellcoder's Handbook* — (shellcode development chapters)
- [Corelan Part 9 — Unicode Exploitation](https://www.corelan.be/index.php/2010/11/07/exploit-writing-tutorial-part-9-writing-unicode-exploits/)
- [epi052/osed-scripts](https://github.com/epi052/osed-scripts) — study the automation approach, then replicate by hand
- [Skape's Shellcode Papers](https://www.hick.org/code/skape/papers/)
- [SLAE 32](https://www.pentesteracademy.com/course?id=3) — Linux-focused but teaches the shellcoding mindset

---

## Phase 5: DEP Bypass with ROP Chains (3–4 weeks)

**Goal: Bypass DEP by constructing ROP chains. The course teaches VirtualAlloc — learn it for understanding, then focus heavily on VirtualProtect and WriteProcessMemory as they are more versatile and cover more real-world scenarios.**

### Topics:
- DEP and NX: what they prevent and what they don't
- Return-Oriented Programming: concept, gadget types, chaining
- Stack pivoting: controlling ESP to point to your ROP chain
- **VirtualAlloc ROP chain** — learn for understanding (course method)
  - Allocates new RWX memory, copies shellcode there, jumps to it
  - Arguments: lpAddress, dwSize, flAllocationType (0x1000), flProtect (0x40)
- **VirtualProtect ROP chain** — drill this
  - Changes protection of existing memory region to RWX
  - Arguments: lpAddress, dwSize, flNewProtect (0x40), lpflOldProtect (writable address)
  - Shellcode stays in place, you just make it executable
- **WriteProcessMemory ROP chain** — drill this
  - Copies shellcode to a code cave in an executable region
  - Arguments: hProcess (-1), lpBaseAddress, lpBuffer, nSize, lpNumberOfBytesWritten
  - Useful when you cannot change memory protections
- Gadget discovery: IDA manual search, ROPgadget, rp++
- Handling bad characters in ROP chains
- Pushad technique for setting up API arguments

### Tasks:
- Build a VirtualAlloc ROP chain following Corelan Part 8
- Build VirtualProtect chains against 3+ different binaries
- Build WriteProcessMemory chains against 3+ different binaries
- Complete all [ROP Emporium](https://ropemporium.com/) challenges (x86)
- Find gadgets both manually in IDA and with automated tools
- Practice stack pivot techniques

### Resources:
- *Practical Reverse Engineering* — Bruce Dang (ROP chapters)
- [ROP Emporium](https://ropemporium.com/)
- [Corelan — ROP Exploitation Series (Part 8+)](https://www.corelan.be/index.php/2010/06/27/exploit-writing-tutorial-part-8-win32-rop/)
- [ForsakenSystems — DEP Bypass Skeleton Templates](https://github.com/ForsakenSystems/.github/blob/main/content/lost-and-found/expl/skel.md)
- [WinDbg Commands Reference](https://learn.microsoft.com/en-us/windows-hardware/drivers/debuggercmds/debugger-commands)
- [Skape's Shellcode Paper (Advanced)](https://www.hick.org/code/skape/papers/)

---

## Phase 6: ASLR Bypass (2–3 weeks)

**Goal: Understand ASLR and bypass it using non-ASLR modules and info leaks.**

### Topics:
- What ASLR randomizes: stack, heap, module base addresses
- Identifying ASLR-enabled vs. non-ASLR modules (lm in WinDbg)
- Partial ASLR bypass: using non-ASLR modules
- Constructing ROP chains from non-ASLR module gadgets
- Information leaks via format strings (bridge to Phase 7)

### Tasks:
- Use lm in WinDbg to list all loaded modules and their base addresses
- Use narly WinDbg extension to check for security headers
- Identify static modules loaded at fixed addresses

### Resources:
- *Practical Reverse Engineering* — Ch. 7 (ASLR internals)
- [Corelan — ASLR Explained & Bypass](https://www.corelan.be/index.php/2009/10/18/exploit-writing-tutorial-part-5-bypassing-aslr/)
- *Windows Internals Part 1* — Image loading and memory layout sections

---

## Phase 7: Format String Specifier Attacks (2–3 weeks)

**Goal: Exploit format string vulnerabilities to build read and write primitives. This is a standalone EXP-301 topic — not a subtopic.**

### Topics:
- Format string basics: %x for reading stack, %n for writing to memory
- Building a read primitive: leaking stack values, module base addresses (ASLR bypass via info leak)
- Building a write primitive: overwriting function pointers, return addresses
- Direct parameter access (%N$x, %N$n) for precise targeting
- Combining format string leaks with ROP chains for full exploitation

### Tasks:
- Leak stack values using %x and %p format specifiers
- Leak a module base address to bypass ASLR
- Write to an arbitrary address using %n with direct parameter access
- Overwrite a function pointer to redirect execution
- Combine a format string info leak with a ROP chain for full DEP+ASLR bypass

### Resources:
- *Hacking: The Art of Exploitation, 2nd Ed* — Jon Erickson (format strings chapter)
- [Format String Vulnerabilities — Aleph One](https://insecure.org/stf/format-string.txt)
- [FuzzySecurity — Stack Smashing 0x07](https://www.fuzzysecurity.com/tutorials/expDev/7.html)
- [Modern Binary Exploitation (RPISEC)](https://github.com/RPISEC/MBE)
- [ROP Emporium — 'write4' and 'badchars' challenges](https://ropemporium.com/)

---

## Phase 8: Full Exploit Chains & Practice (4–6 weeks)

**Goal: Combine everything into end-to-end exploits. RE in IDA → crash triage in WinDbg → exploit development → shell. Practice under time pressure and document as you go.**

### Topics:
- Full chain: Crash → root cause analysis → offset calculation → SEH/stack overflow → DEP bypass (ROP) → shellcode injection → execution
- Combining ASLR bypass (non-ASLR module or format string leak) with DEP bypass (ROP) with custom shellcode
- Writing exploit automation scripts in Python 3
- Report writing: documenting every step with screenshots

### Practice Targets:
- Vulnserver (multiple commands test different techniques)
- FreeFloat FTP
- Any vulnerable binary with DEP enabled + a non-ASLR module
- **EXP-301 Challenge Labs (3 labs)** — treat as dress rehearsals

### Tasks:
- Pick a vulnerable binary — RE it in IDA, find the bug, build a working exploit
- Build a VirtualProtect ROP chain against a new binary with DEP enabled
- Build a WriteProcessMemory ROP chain against a different binary
- Write a complete dropper shellcode from scratch without referencing notes
- Write custom boofuzz fuzzer for a target app
- Build a Python exploit template with modular sections
- Complete all 3 EXP-301 Challenge Labs independently

### Resources:
- *The IDA Pro Book* — Chris Eagle (full analysis section)
- [FuzzySecurity — Exploit Dev Tutorials](https://www.fuzzysecurity.com/tutorials.html)
- [epi052/osed-scripts](https://github.com/epi052/osed-scripts)

---

## Phase 9 (Bonus): x64 Shellcode & VM Escape (Post-Pass)

**Goal: Extend into x64 exploitation and hypervisor escape techniques. These are my additions beyond the core EXP-301 curriculum — practical skills for a world that has largely moved past 32-bit.**

### Topics:
- x64 calling convention differences (RCX, RDX, R8, R9 for first 4 args; shadow space)
- x64 shellcode development
- VM escape concepts and hypervisor attack surfaces

### Resources:
- TBD