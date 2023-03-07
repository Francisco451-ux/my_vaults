[[Stack-Based Buffer Overflows on Linux x86]]
DThese include security mechanisms:

-   `Canaries`
-   `Address Space Layout Randomization` (`ASLR`)
-   `Data Execution Prevention` (`DEP`)


## Canaries
The `canaries` are known values written to the stack between buffer and control data to detect buffer overflows. The principle is that in case of a buffer overflow, the canary would be overwritten first and that the operating system checks during runtime that the canary is present and unaltered.

## Address Space Layout Randomization (ASLR)

Address Space Layout Randomization (`ASLR`) is a security mechanism against buffer overflows. It makes some types of attacks more difficult by making it difficult to find target addresses in memory. The operating system uses ASLR to hide the relevant memory addresses from us. So the addresses need to be guessed, where a wrong address most likely causes a crash of the program, and accordingly, only one attempt exists.

## Data Execution Prevention (DEP)

`DEP` is a security feature available in Windows XP, and later with Service Pack 2 (SP2) and above, programs are monitored during execution to ensure that they access memory areas cleanly. DEP terminates the program if a program attempts to call or access the program code in an unauthorized manner.