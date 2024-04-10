#### racecar-CTF

## CHALLENGE DESCRIPTION<br />
Did you know that racecar spelled backwards is racecar? Well, now that you know everything about racing, win this race and get the flag!<br />

The CTF provided a zip file that can be unzipped using a password *hackthebox*<br />
I used the command **unzip racecar.zip** and the password to unzip the file.<br />


After unzipping the file I obtained one file which is an executable named racecar.<br /> 
I decided to run the file but after providing answers to the few options provided after executing it<br /> I failed to obtain the flag.<br />

I then decided to see the source code of the executable by opening it in **Ghidra**.<br />
An alternative is to use  [decompiler explorer](www.dogbolt.org) which is an online resource,<br /> if you don't have a disassembler locally installed.

After examining the code in Ghidra i found a main function and in it there were two functions car_menu() and car_selection().<br />
Carefully examining the functions revealed that the car_menu() is the function i should pay more attention to.<br />
The code also revealed that the code was vulnerable to a **format string attack**.<br />For more information on this exploit kindly visit [format string attack info](https://owasp.org/www-community/attacks/Format_string_attack).<br />

I created two files main.c and car_menu.c which have the code for the main() function and car_menu() function respectively.<br />
You can carefully examine it for studying purpose, but to keep it short, basically it’s:.<br />
* If you choose car 1 and race 2, or car 2 and race 1, you win.<br />
* When you win, it let you input something there, and print it back out.<br />
After winning,the code then required me to input some data. Which would be used to produce the flag but encoded in hexadecimal format.<br />
I then had to decode the flag to get the actual readable flag..<br />

The code also required a file named flag.txt as indicated in the car_menu.py file.<br />

Since a flag.txt had to be passed to the stream as i had read while analysing the code,<br /> 

I created a flag.txt file and added some data that would be easy to identify in hexadecimal format, in this case  AAAAA.<br />
After doing this I ran the executable file again and this time after winning the race I passed %p as the data multiple times.<br />
This was the actual process of using **exploit strings** in the **format string attack** to exploit the vulnerability.<br />

This produced a hexadecimal string.<br />
*0x574361c0 0x170 0x56555d85 0x1 0x5e 0x26 0x1 0x2 0x5655696c 0x574361c0 0x57436340 0x7b425448 0x5f796877 0x5f643164 0x34735f31 0x745f3376 0x665f3368 0x5f67346c 0x745f6e30 0x355f3368 0x6b633474 0x7d213f 0xf149d800 0xf7f9e3fc 0x56558f8c 0xfffe9b38 0x56556441 0x1 0xfffe9be4 0xfffe9bec*.<br />

I then wrote a script to decode this in a **decode.py** file which when run using the command python3 decode.py produced the flag.

Since it took me a few tries to get the correct exploit string as I was doing it manually, I did some research and discovered there was an automated way to find the flag using pwntools..<br />

I would like to appreciate [remoteshell.zip](https://remoteshell.zip/racecar/) for the detailed walkthrough of the challenge..<br />
The code to the automated script can be found on the **exploit.py** file


