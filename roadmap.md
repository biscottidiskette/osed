# OSED Prep Roadmap

## Phase 0: Foundation Cleanup (1–2 weeks)

### Topics:
<ul>
    <li>x86 Assembly (focus on 32-bit)</li>
    <li>Calling conventions and stack layout</li>
    <li>PE file format basics</li>
    <li>Virtual memory and Windows API</li>
</ul>

### Tools:
<ul>
    <li>WinDbg Preview</li>
    <li>IDA Free or IDA Pro</li>
</ul>

### Tasks:
<ul>
    <li>Trace function prologs/epilogs in WinDbg</li>
    <li>Use IDA to identify entry points, strings, function calls</li>
    <li>Explore VirtualAlloc, LoadLibrary, etc., in memory</li>
</ul>

### Resources:
<ul>
    <li>Practical Reverse Engineering – Bruce Dang (Ch. 1–3)</li>
    <li>Windows Internals, Part 1 – Mark Russinovich (Ch. 1–4)</li>
    <li><a href="https://opensecuritytraining.info/IntroX86.html">OpenSecurityTraining – Intro to x86</a></li>
    <li>The IDA Pro Book – Chris Eagle (Ch. 1–3)</li>
    <li><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools">Microsoft Learn – WinDbg Preview Guide</a></li>
</ul>

## Phase 1: Basic Exploitation + SEH (2–3 weeks)

### Topics:
<ul>
    <li>Stack-based overflows (manual EIP control)</li>
    <li>SEH overwrite technique</li>
    <li>Bad character handling</li>
    <li>Egghunters</li>
</ul>

### Tools:
<ul>
    <li>WinDbg: Breakpoints, memory inspection, !exploitable</li>
    <li>IDA: String analysis, function mapping</li>
    <li>Python: Pattern generation & automation scripts</li>
</ul>

### Tasks:
<ul>
    <li>Fuzz Vulnserver, SLMail, etc.</li>
    <li>Manually find offsets using cyclic patterns</li>
    <li>Trigger and redirect execution via SEH overwrite</li>
</ul>

### Resources:
<ul>
    <li><a href="https://www.corelan.be/index.php/articles/">Corelan Exploit Writing Part 1–3 (Manual SEH)</a></li>
    <li>The Shellcoder's Handbook – Jack Koziol (Buffer overflow chapters)</li>
    <li><a href="https://www.youtube.com/playlist?list=PLhixgUqwRTjxglIswKp9mpkfPNfHkzyeN">LiveOverflow – Stack Overflow & Exploitation</a></li>
    <li><a href="https://www.fuzzysecurity.com/tutorials.html">FuzzySecurity – Exploit Dev Tutorials</a></li>
    <li><a href="https://github.com/jtpereyda/boofuzz">Boofuzz GitHub</a></li>
</ul>

## Phase 2: DEP Bypass + ROP (3–4 weeks)

### Topics:
<ul>
    <li>DEP and NX concepts</li>
    <li>ROP chain construction and validation</li>
    <li>VirtualAlloc and pivoting</li>
    <li>Gadget discovery and chaining</li>
</ul>

### Tools:
<ul>
    <li>WinDbg: Set breakpoints, analyze stack pivots</li>
    <li>IDA: Manual gadget identification and verification</li>
    <li>Python: ROP chain builder and encoder</li>
</ul>

### Tasks:
<ul>
    <li>Build ROP chains from IDA-only gadgets</li>
    <li>Validate stack behavior and shellcode allocation in WinDbg</li>
    <li>Deploy shellcode via controlled memory regions</li>
</ul>

### Resources:
<ul>
    <li>Practical Reverse Engineering – Bruce Dang (ROP chapters)</li>
    <li><a href="https://ropemporium.com/">ROP Emporium (x86 Concepts)</a></li>
    <li><a href="https://www.corelan.be/index.php/2010/06/27/exploit-writing-tutorial-part-8-win32-rop/">Corelan – ROP Exploitation Series</a></li>
    <li><a href="https://github.com/JonathanSalwan/ROPgadget">ROPgadget Tool</a></li>
    <li><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debuggercmds/debugger-commands">WinDbg Commands Reference</a></li>
</ul>

## Phase 3: ASLR Awareness & Partial Bypass (2–3 weeks)

### Topics:
<ul>
    <li>What ASLR does (module randomization)</li>
    <li>How ASLR behaves with system vs. non-system modules</li>
    <li>Static vs. dynamic linking (ASLR implications)</li>
    <li>Partial ASLR bypass via non-ASLR modules (e.g., loaded DLLs)</li>
    <li>Return-to-non-ASLR (ret2lib) techniques</li>
</ul>

### Tools:
<ul>
    <li>WinDbg: Use !address, lm to inspect ASLR-enabled modules</li>
    <li>IDA: Analyze binary headers and imports for ASLR compliance</li>
    <li>procmon or PE-bear to inspect DLL loading behavior</li>
</ul>

### Tasks:
<ul>
    <li>Identify ASLR-enabled vs non-ASLR modules</li>
    <li>Build ROP chains using non-ASLR modules</li>
    <li>Patch binaries to toggle ASLR for practice</li>
    <li>Automate ASLR state analysis in WinDbg</li>
</ul>

### Resources:
<ul>
    <li>Practical Reverse Engineering – Ch. 7 (ASLR internals)</li>
    <li><a href="https://www.corelan.be/index.php/2009/10/18/exploit-writing-tutorial-part-5-bypassing-aslr/">Corelan – ASLR Explained</a></li>
    <li><a href="https://learn.microsoft.com/en-us/sysinternals/">Windows Internals Part 1* – Section on image loading and memory layout</a></li>
    <li><a href="https://learn.microsoft.com/en-us/windows/win32/secbp/address-space-layout-randomization">Microsoft Docs – ASLR Overview</a></li>
    <li><a href="https://github.com/hasherezade/pe-bear">PE-bear</a> – View PE ASLR flags</li>
</ul>

## Phase 4: Complex Payloads & Constraints (3–4 weeks)

### Topics:
<ul>
    <li>XOR and alphanumeric encoding</li>
    <li>Unicode shellcode techniques</li>
    <li>Staged payloads</li>
    <li>Heap spraying introduction</li>
</ul>

### Tools:
<ul>
    <li>IDA: Manual shellcode inspection & decoding</li>
    <li>WinDbg: Memory dump comparison and shellcode tracing</li>
    <li>Python: Custom shellcode encoders</li>
</ul>

### Tasks:
<ul>
    <li>Write XOR encoder with decoder stub</li>
    <li>Create alphanumeric payloads using known instructions</li>
    <li>Deliver staged shellcode through SEH with character constraints</li>
</ul>

### Resources:
<ul>
    <li><a href="https://www.corelan.be/index.php/2009/07/25/writing-buffer-overflow-exploits-a-quick-and-basic-tutorial/">Corelan – Shellcode & Encoding</a></li>
    <li><a href="https://www.youtube.com/playlist?list=PLhixgUqwRTjz5D08ezs6cjhz1IC1o6wzA">LiveOverflow – Shellcode Writing & Encoding</a></li>
    <li><a href="https://github.com/corelan/mona">Shellcode Encoder Repo (alphanumeric/unicode)</a></li>
    <li>The Shellcoder's Handbook – (Shellcode development chapters)</li>
</ul>

## Phase 5: Full Exploit Chains & Simulation (4–6 weeks)

### Scenario Labs:
<ul>
    <li>Crash → SEH → DEP bypass → Shellcode injection → Shell</li>
    <li>Unicode/ASCII-only ROP shellcode chains</li>
    <li>Manual analysis and patching in IDA</li>
</ul>

### Tools:
<ul>
    <li>IDA: Static reversing, API identification, gadget discovery</li>
    <li>WinDbg: Dynamic debugging, crash triage, shellcode tracing</li>
    <li>Python: Full exploit automation</li>
</ul>

### Tasks:
<ul>
    <li>Full exploit chain (including egghunter or encoder)</li>
    <li>Manual shellcode injection into allocated memory</li>
    <li>Develop automation script to validate exploit success</li>
</ul>

### Resources:
<ul>
    <li>The IDA Pro Book – Chris Eagle (full analysis section)</li>
    <li><a href="https://github.com/sam-b/hacksys-extreme-vulnerable-driver">HackSys Extreme Vulnerable Driver (HEVD) – Practice memory corruption in kernel mode</a></li>
    <li><a href="https://www.offensive-security.com/offsec/osed/">Offensive Security's Public OSED Blogs</a></li>
    <li><a href="https://www.fuzzysecurity.com/tutorials.html">FuzzySecurity – OSED Exploit Tutorials</a></li>
    <li><a href="https://www.codemachine.com/article_windbg.html">Windbg Tips: Advanced Usage</a></li>
</ul>




