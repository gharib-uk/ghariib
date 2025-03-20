---
title: "Sandbox Rust Development with Rust Analyzer"
date: Sun, 25 Oct 2020 12:47:18 GMT
draft: false
type: posts
categories: 
- 
---
# Sandbox Rust Development with Rust Analyzer

<br/>

<br/>
A little over a year ago, I created a simple python script for sandboxing Rust development using Docker. It’s called [saferrust](https://gitlab.com/mikecardwell/saferrust). You basically copy it into your PATH, name it “cargo” and pretend like you have the real cargo application installed. It pulls down the [latest offical Rust docker image from dockerhub](https://hub.docker.com/_/rust), mounts your CWD into it and then runs your cargo commands inside the docker container. I did this basically because I wanted to secure my homedir against potentially malicious packages on [crates.io](https://crates.io/) during development. If you want to know more about it, [check out the project on gitlab](https://gitlab.com/mikecardwell/saferrust).

To be honest, I haven’t done much Rust development since then, so there are probably a bunch of pain points that I’ve not handled. I’m starting a new project though, and thought I’d try out the language server [rust-analyzer](https://github.com/rust-analyzer/rust-analyzer) as I’ve heard good things about it.

Ordinarily with [Visual Studio Code](https://code.visualstudio.com/) (my IDE of choice), you can just install the rust-analyzer extension and begin. However, in my case as I don’t have rust installed directly on my host, there were a few extra steps to take.

First of all, the VS Code extension tries to install and run the rust-analyzer binary, and fails. So I created another wrapper script named rust-analyzer and dropped it in my PATH:

```
#!/usr/bin/env bash
set -e

if [ "$USER" != "root" ]; then
    sudo "$0" "${@:1}"
    exit 0
fi

IMAGE_NAME="grepular/rust-analyzer"

# Build the image if it does not exist
if [[ $(docker images --filter "reference=$IMAGE_NAME" -q) == "" ]]; then
    docker build -q -t "$IMAGE_NAME" . -f-<<EOF
FROM rust:latest
RUN rustup component add rust-src
RUN curl -L https://github.com/rust-analyzer/rust-analyzer/releases/latest/download/rust-analyzer-linux -o /rust-analyzer
RUN chmod 755 /rust-analyzer
ENTRYPOINT ["/rust-analyzer"]
EOF
fi

docker run \
  -u $(stat -c '%u:%g' .) \
  -i --rm \
  -v "$PWD:$PWD:ro" \
  --workdir "$PWD" \
  "$IMAGE_NAME" "$@"
```

I then had to go into the VS Code settings and manually set the path to this script:

```
{
  "rust-analyzer.serverPath": "/usr/local/bin/rust-analyzer"
}
```

You’ll notice at the beginning of the script, it re-runs it’s self under sudo if it’s not already running as root. This is because my user does not have access to the docker socket (for security reasons). And although my user does have sudo access, it is password protected. Because of the password protection, the rust-analyzer extension fails to run because it is prompted for a password and wasn’t expecting that. So, I placed an exception in my sudoers config to allow my user to run the rust-analyzer script using sudo, without a password. I did this by adding the following to /etc/sudoers.d/mike-rust-analyzer:

```
mike ALL=NOPASSWD: /usr/local/bin/rust-analyzer *
```

And there we have it. I can continue to run cargo inside a sandboxed environment, and use rust-analyzer inside another sandboxed environment, whilst using my IDE on the host to do development.

P.S. Initial impressions of rust-analyzer are that it’s very good and I never want to do Rust development again without it.

[![Programming Rust](https://www.grepular.com/images/amazon/programming_rust.jpg)](https://www.grepular.com/redir?key=amazon_programming_rust "Programming Rust") [![Docker: Up and Running](https://www.grepular.com/images/amazon/docker_up_and_running.jpg)](https://www.grepular.com/redir?key=amazon_docker_up_and_running "Docker: Up and Running")

#### [Source](https://www.grepular.com/Sandbox_Rust_Development_with_Rust_Analyzer)

<br/>
---
