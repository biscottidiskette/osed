# Phase 7: Payload Encoding & Constraints

## Badchar Handling
<ul>
    <li>Write badchar detection script using memory dump from WinDbg</li>
    <li>Confirm which characters break shellcode execution</li>
</ul>

## Encoding & Unicode
<ul>
    <li>Use msfvenom -e x86/shikata_ga_nai and test</li>
    <li>Write XOR encoder/decoder in Python</li>
    <li>Reproduce Corelan ASCII/Unicode SEH exploit:</li>
    <ul>
        <li><a href="https://www.corelan.be/index.php/2010/11/07/exploit-writing-tutorial-part-9-writing-unicode-exploits/">Reference: Corelan Part 9: Unicode Exploitation</a></li>
    </ul>
</ul>