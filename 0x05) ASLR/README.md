# Phase 5: ASLR Identification & Bypass

## ASLR Detection
<ul>
    <li>Use lm in WinDbg to list loaded modules and addresses</li>
    <li>Use PE-bear to confirm ASLR flag (/DYNAMICBASE)</li>
</ul>

## Partial ASLR Bypass
<ul>
    <li>Identify static modules (e.g., libeay32.dll) loaded with fixed addresses</li>
    <li>Construct ROP chain using non-ASLR binary</li>
    <li><a href="https://www.corelan.be/index.php/2009/10/18/exploit-writing-tutorial-part-5-bypassing-aslr/">Read: Corelan ASLR Tutorial</a></li>
</ul>