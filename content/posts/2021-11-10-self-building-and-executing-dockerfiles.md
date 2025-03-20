---
title: "Self Building and Executing Dockerfiles"
date: Wed, 10 Nov 2021 20:30:52 GMT
draft: false
type: posts
---
# Self Building and Executing Dockerfiles

<br/>

<br/>
Sometimes there are programs that I want to use, and they’re either missing from my OS package repository, or I want to run a later version. I’ve started using a little trick to make these available on my systems. Here is a copy of my ~/bin/youtube-dl

```
#!/usr/bin/env -S bash -c "podman run --rm -w /x -v "\$PWD:/x" \$(podman build -q - < \$0) \${@:1}"
FROM python:3-slim
RUN python3 -m pip --no-cache-dir install yt-dlp
ENTRYPOINT ["yt-dlp"]
```

It is a valid Dockerfile, but if I run it, it will build an image using podman containing the fork of youtube-dl that I want to use (yt-dlp), and then run that image, with the args I supplied. In the above example it mounts my working directory into the container so that downloaded files reach the host.

Of course, I could have made a shell script as follows instead:

```
#!/usr/bin/env bash
podman build -q -t "localhost/youtube-dl" -<<EOF &>/dev/null
FROM python:3-slim
RUN python3 -m pip --no-cache-dir install yt-dlp
ENTRYPOINT ["yt-dlp"]
EOF

podman run --rm -w /x -v "$PWD:/x" -w /x "localhost/youtube-dl" "$@"
```

Some might even argue that it’s more readable. But then I have to worry about escaping parts of the inline Dockerfile, and I lose my IDE’s Dockerfile linting support too.

#### [Source](https://www.grepular.com/Self_Building_and_Executing_Dockerfiles)

<br/>
---
