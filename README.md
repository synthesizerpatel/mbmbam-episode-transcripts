# mbmbam-episode-transcripts
OpenAI's Whisper transcoded MBMBAM episode transcripts

I wrote a tool that spidered through the MBMBAM episode list on maximumfun.com and got each episode URL.
Then I ran

```
for x in $(cat urls); do 
    youtube-dl "$x"
done
````

Once I downloaded all the episodes I let OpenAI Whisper loose on the episodes using the large data set for a week.

This resulted in the *.srt, *.tyxt, and *.vtt files generated for each episode. 

I retained the original mp3 filenames. They're a bit unwieldy. 
