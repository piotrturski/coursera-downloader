# Coursera downloader

Helps download mp4s and pdfs from courses that require log-in.

#### Usage:

1. Create empty directory. You will save files there
1. Log-in to Coursera using browser
1. Save course page with list of lectures as a htm/html file
1. Save browser cookies as a txt file (in curl acceptable format)
1. You should have exactly 2 files in a directory: one htm(l) and one txt. In that directory run in bash:

   ```shell
cat *.htm@(l|) | # htm, html \
grep -oiE "(https://[^\"]*download.mp4[^\"]*)|(https://[^\"]*\.pdf)" | # pdf, mp4 \
xargs -n 1 -P 8 curl --compressed -sSJOL --cookie *.txt && # cookies in txt \
rename 'use URI::Escape; $_ = uri_unescape $_; s/\//_/g' * # decode uri, change / to _
```

#### Saving cookies

Curl 7.35.0 accepts plain HTTP headers or the Netscape/Mozilla cookie file format. From FF you can export cookies with one click using e.g. https://addons.mozilla.org/pl/firefox/addon/export-cookies/

#### Safely remove cookies & html

```shell
srm -vd *.txt *.htm@(l|)
```

#### Subpages:

Some courses on main page contain only iframes locations and each iframe contains url of its mp4. In this case use grep to find all iframes locations, store them in a file, log in to coursera and save cookies. This script will save content of all iframes (inlcuding urls of mp4s) to iframes.txt
```
wget -i links_from_main_page.txt -O iframes.txt --load-cookies /tmp/cookies.txt 
```
