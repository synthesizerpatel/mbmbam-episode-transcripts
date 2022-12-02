# mbmbam-episode-transcripts
OpenAI's Whisper transcoded MBMBAM episode transcripts

## Description:

This git repo has OpenAI Whisper generated time-coded transcripts for every episode of MBMBAM in .srt, .txt, and .vtt formats. 
I'm aware that there are transcripts available through the webpage but they're in PDF format and they lack time code. They are
a wonderful resource, just not the type of resource I needed. Hence, this project.

I used the large dataset for Whisper which seems fairly high quality.

## Copyright

I place this work into the public domain. You can do whatever you want with it. If you do something cool I'd like to hear about it.

## Methodology:

I wrote a tool that quick and dirty spider to gather all the MBMBAM episode base URLS
```
#!/usr/bin/env python3

import requests
from lxml import etree
import lxml.html
import re
import pprint

MAX_PAGES=54 # This is how many pages of episode data there are.

forms = [
        # I anticipated more than one form of URL but there didn't turn out to be.
        re.compile('https:\/\/maximumfun.org\/episodes\/my-brother-my-brother-and-me/') # 636-millie-vanilli-bobby-flay/
]

urls = {}


for x in range(1,MAX_PAGES):
        r = requests.get('https://maximumfun.org/podcasts/my-brother-my-brother-and-me/?_post-type=episode&_paged={0}'.format(x))
        x = lxml.html.fromstring(r.content)
        for y in x.xpath('//a/@href'):
                m = forms[0].match(y)
              
                if m is not None:
                        # De-dupe the base URLs
                        urls[y] = 1

for x in urls.keys():
        print(x)
```

Then I parsed the de-dupped list and youtube-dl'd each episode

```
for x in $(cat urls); do 
    youtube-dl "$x"
done
```

Once I downloaded all the episodes I let OpenAI Whisper loose on the episodes using the large data set for a week on my Nvidia 3090. 
I don't recommend it.

```
for x in *.mp3; do
     whisper --model large $x --language English
done
```

This resulted in the *.srt, *.txt, and *.vtt files generated for each episode. 

I retained the original mp3 filenames. They're a bit unwieldy. 
