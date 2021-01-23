# bufferoverflow
Scripts to help with buffer overflow

## exploit.py
The final script that will exploit the vulnerable.exe program.

## fuzzer.py
It connects and fuzz the vulnerable binary with values increasing from 100 untill it crashes to find how many characters the program accept before it breaks.

## findbadchar.py
Generate a bytearray using mona, and exclude the null byte (\x00) by default. Note the location of the bytearray.bin file that is generated (if the working folder was set per the Mona Configuration section of this guide, then the location should be C:\mona\oscp\bytearray.bin).
```
!mona bytearray -b "\x00"
```
Now generate a string of bad chars that is identical to the bytearray. The following python script can be used to generate a string of bad chars from \x01 to \xff.
```
python findbadchar.py
```
Update your exploit.py script and set the payload variable to the string of bad chars the script generates.
Restart vulnerable.exe in Immunity and run the modified exploit.py script again. Make a note of the address to which the ESP register points and use it in the following mona command:
```
!mona compare -f C:\mona\oscp\bytearray.bin -a <address>
```
A popup window should appear labelled "mona Memory comparison results".
Not all of these might be badchars! Sometimes badchars cause the next byte to get corrupted as well, or even effect the rest of the string.
The first badchar in the list should be the null byte (\x00) since we already removed it from the file. Make a note of any others. Generate a new bytearray in mona, specifying these new badchars along with \x00. Then update the payload variable in your exploit.py script and remove the new badchars as well.
Restart vulnerable.exe in Immunity and run the modified exploit.py script again. Repeat the badchar comparison until the results status returns "Unmodified". This indicates that no more badchars exist.
