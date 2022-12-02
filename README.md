# mbmbam-episode-transcripts
OpenAI's Whisper transcoded MBMBAM episode transcripts

I wrote a tool that naive spider to gather all the episode base URLS
```
#!/usr/bin/env python3

import requests
from lxml import etree
import lxml.html
import re
import pprint

MAX_PAGES=54

forms = [
        re.compile('https:\/\/maximumfun.org\/episodes\/my-brother-my-brother-and-me/') # 636-millie-vanilli-bobby-flay/
]

urls = {}


for x in range(1,MAX_PAGES):
        r = requests.get('https://maximumfun.org/podcasts/my-brother-my-brother-and-me/?_post-type=episode&_paged={0}'.format(x))
        x = lxml.html.fromstring(r.content)
        for y in x.xpath('//a/@href'):
                m = forms[0].match(y)
                if m is not None:
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
