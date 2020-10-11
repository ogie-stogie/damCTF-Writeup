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
*The counter is off in the picture because I added a counter near the end of the run since it was taking a couple minutes. Luckily, the last page was stored as a environment variable so I didn't lose progress.<br>
Here is the code:<br>
<br>
```
export DAM_COUNT=0; while true; do export DAM_CTF_URL=$(curl -s https://finger-warmup.chals.damctf.xyz/$DAM_CTF_URL | cut -d'"' -f 2); curl -s https://finger-warmup.chals.damctf.xyz/$DAM_CTF_URL | grep dam; export DAM_COUNT=$(echo "$DAM_COUNT+1" | bc) ;echo $DAM_COUNT; done
```
<br>
<br>

