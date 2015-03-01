# Coursera downloader

Helps download mp4s and pdfs from courses that require log-in to show list of lectures but not individual videos.

#### Usage:

Save course page with list of lectures as a html file, say <em>index.html</em>. This file should be the only file in a directory. Then in that directory run

```
cat * | grep  -oiE "(https://.*download.mp4[^\"]*)|(https://.*\.pdf)" | \
xargs -n 1 -P 8 curl --compressed -sSJOL
```

To cleanup encoded filenames run
```
rename 'use URI::Escape; $_ = uri_unescape $_' *
```

#### Limitations:

Won't clean filenames containing `%2F` (`/`).

#### Subpages:

Some courses on main page contain only iframes locations and each iframe contains url of its mp4. In this case use grep to find all iframes locations, store them in a file, log in to coursera and save cookies. This script will save content of all iframes (inlcuding urls of mp4s) to iframes.txt
```
wget -i links_from_main_page.txt -O iframes.txt --load-cookies /tmp/cookies.txt 
```
