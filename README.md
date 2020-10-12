# damCTF-Writeup
[Write-Up Link](https://github.com/tbart27/damCTF-Writeup/blob/main/README.md)

### Team: 466 Crew
Taylor Bart<br>
Matt Evans<br>
John Tiffany<br>
Haytham Mouti<br>
<br>

### web/finger-warmup
As a team we were able to solve this flag in two different ways. Matt and I began working on this one and we quickly figured out that the trick for this challenge was to keep getting the next link until the html we recieved contained the flag. Here is an example of the html we were given:<br>
<br>
![](https://github.com/tbart27/damCTF-Writeup/blob/main/web1.png)<br>
<br>
Matt ended up solving this challenge a couple minutes before my program finished running but I'll leave the description of his methods for his report since we both individually came to these solutions.<br>
For my method, I knew that I could curl for each page and then parse it for what was between the quotation marks. This variable would then be saved and used in another curl call to the dam CTF server. With each result I would then grep for dam and if successful this would print the flag (I later added a counter to print just for a sanity check). From Matt's and my results, we could see that we had to iterate through 1000 pages before we were given the flag. Here is my result:<br>
<br>
![](https://github.com/tbart27/damCTF-Writeup/blob/main/web2.png)<br>
<br>
*The counter is off in the picture because I added a counter near the end of the run since it was taking a couple minutes. Luckily, the last page was stored as a environment variable so I didn't lose progress.*<br>
Here is the code:<br>
<br>
```
export DAM_COUNT=0; while true; do export DAM_CTF_URL=$(curl -s https://finger-warmup.chals.damctf.xyz/$DAM_CTF_URL | cut -d'"' -f 2); curl -s https://finger-warmup.chals.damctf.xyz/$DAM_CTF_URL | grep dam; export DAM_COUNT=$(echo "$DAM_COUNT+1" | bc) ;echo $DAM_COUNT; done
```
### pwn/allokay
After finger-warmup, I joined John on the allokay pwn challenge. From testing input, we were able to find an interesting pattern where:<br>
<br>
![](https://github.com/tbart27/damCTF-Writeup/blob/main/pwn1.png)<br>
<br>
For every multiple of 16 that favorite number increased, we were able to input an additional 2 numbers. This looked like a good buffer overflow vulnerability until after looking at the binary in binary ninja, we noticed that favorite number had to be less than 100. We saw that it was making an unsigned comparison, however simple testing for negative numbers for negative number returned an erroneous "Huh?" from the binary.<br>
<br>
![](https://github.com/tbart27/damCTF-Writeup/blob/main/pwn2.png)<br>
<br>
Jack and I proceed to spend the majority of this CTF trying to overflow the buffer for the "Please enter the numbers!" section. Binary ninja allowed us to see a potential vulnerability in this buffer but when we used gdb to analyze the stack. We noticed that only the first number entered was being saved to the buffer address on the stack. Various testing and gdb analysis showed that we were unable to overflow the buffer with any of our methods. Jack then mentioned that the hint for the challenge was "Time to ROP!". In the final moments of the CTF, I decided to install ROPgadget and see if we have all the gadgets necessary to build a rop-chain. Sadly, there was a missing gadget as seen below:<br>
<br>
![](https://github.com/tbart27/damCTF-Writeup/blob/main/pwn3.png)<br>
<br>
If we had more time, I would've continued testing for unsigned/signed error overflows and I would've continued to see if there was a workaround to build a rop-chain.<br>
<br>
### Conclusion
After this somewhat disappointing CTF, I realized that penetration software is becoming more prevalent in the challenges. In an unrelated project, I have installed open-jdk so I will be looking into installing Ghidra as a backup for when Binary ninja leads us to a dead end. Moreover learning pwndbg is still a work in progress, however it is becoming more manditory since regular GDB analysis is taking an exorbitant amount of time to perform. Lastly, I attempted malware/phase near the end of the CTF but had to use browser software since I didn't have wireshark (credit to John for reminding me about this software!) set up properly to analyze the pcapng file.
