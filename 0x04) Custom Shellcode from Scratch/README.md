# Phase 4: DEP Bypass + ROP Chains

## DEP Mechanics
<ul>
    <li>Enable DEP in test app, verify crash with WinDbg: !address, !exploitable</li></li>
    <li>Read: PRE Chapter 2 – Windows Memory Management</li>
    <li><a href="https://www.corelan.be/index.php/2010/06/27/exploit-writing-tutorial-part-8-win32-rop-stackpivoting/">Watch: Corelan ROP Series Part 1</a></li>
</ul>

## ROP Chains
<ul>
    <li>Build a ROP chain to call VirtualAlloc with correct arguments</li>
    <li>Use ROPgadget or IDA to locate gadgets in non-ASLR DLL</li>
    <li>Validate control of stack after pivot</li>
    <li>Trigger execution of shellcode in newly allocated memory</li>
</ul>