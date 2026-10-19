# Phase 2: Stack BOF & SEH Exploits

## Vanilla Buffer Overflow
<ul>
    <li>Use pattern_create + pattern_offset to get EIP control</li>
    <li>Reproduce Vulnserver TRUN BOF manually</li>
    <ul>
        <li><a href="https://www.fuzzysecurity.com/tutorials/expDev/1.html">Reference: FuzzySecurity Stack 0x01</a></li>
    </ul>
    <li>Use WinDbg to verify ESP, EBP, shellcode address</li>
</ul>

## SEH Overwrite
<ul>
    <li>Reproduce SEH exploit in GMON or LTER from Vulnserver</li>
    <ul>
        <li><a href="https://www.corelan.be/index.php/2009/09/13/exploit-writing-tutorial-part-2-seh/">Reference: Corelan Exploit Writing Part 2</a></li>
    </ul>
    <li>Use badchar generation script to validate shellcode integrity</li>
    <li>Create an egghunter to bypass small shellcode buffer</li>
</ul>

## Shellcode Execution
<ul>
    <li>Generate staged reverse shell with msfvenom -f c</li>
    <li>Inject into process manually via debugger</li>
</ul>