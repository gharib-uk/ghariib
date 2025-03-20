---
title: "Abusing Dockers Remote APIs"
date: 2014-09-09T00:39:00.000-07:00
draft: false
type: posts
categories: 
- API,docker,pentest,remote
---
# Abusing Dockers Remote APIs

<br/>

<br/>
### Forewords: is this post about a security vulnerability?

Ultimately it's not. This is a short note on how to exploit a somehow under-documented feature in the [Docker](https://www.docker.com/) remote APIs, since I did not manage to find clear guidance elsewhere and had to experiment with it myself. The reason for sharing is to save you time during the next pentest. That said, do I think this is bad? Yes, I do, as I will explain later on.  
  
**EDIT:**  It turns out maybe I wasn't so wrong worrying about this.  
See [this announcement](https://groups.google.com/forum/#!msg/docker-announce/aQoVmQlcE0A/smPuBNYf8VwJ).  
  

### So, what is this about?

TL;DR; The Docker's Remote APIs trivially allow anyone with access to them to obtain **access to the file system of the host**, by design, and are unauthenticated (but disabled) by default.

  

Docker is a container-based platform for application "shipment", but to be fair, [the official what is Docker page](https://www.docker.com/whatisdocker/) does a better job at explaining what this is all about. Docker is all the rage in these days and if you have never heard of it you should really look it up. It's quite likely to show up in one of your next penetration tests since companies seem to be experimenting with it quite extensively.

  

Docker is usually managed via a command line tool, which **connects to the Docker server via a Unix socket**. Access is restricted to the root user by default or to an aptly named Docker group, at least on the [Ubuntu packages](https://docs.docker.com/installation/ubuntulinux/) I've experimented with. So far, so good (sort of, but I don't really have strong arguments here).

  

This is of course not very convenient if you are working in any environment but your bedroom, so the helpful devs have provided a RESTful API which can be bound to any HTTP port. It is **not enabled by default**, which is something worth stressing, but can be turned on via a flag (\-H tcp://IP\_ADRESS:PORT). [IANA](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml?search=docker) has assigned ports 2375 and 2376 to the cleartext and SSL protected versions of the APIs respectively.

  

Looking at the [API reference](https://docs.docker.com/reference/api/docker_remote_api/) you'll see the APIs support all the operations you'd expect in a VM-management tool. By default there is no authentication so all you need is finding the target (which you can fingerprint by hitting the /version endpoint) - unless, of course, the admin has enabled some other kind of protection. The simplest way to do so is to use Docker's [tlsverify feature](https://docs.docker.com/articles/https/), and everyone should do that. However, given the process does not work on OsX, guess how many programmer's laptops you are going to pop?

  

### Accessing the host file system

The trick is ultimately quite simple: you just start a new container (think of it as a VM and you won't be far off) with a special configuration, then access it. First, create a container on the host (I'm trying to keep the call small here):

  

POST /containers/create?name=cont\_name HTTP/1.1

Content-Type: application/json

    {

      "Cmd": \["/bin/bash"\],

      "Image":"ubuntu",

    }

  

You'll get back an ID for your container. Now, start the container: note that in my experiment I was able to make these "Binds" only work once, the first time a container is started.

  

POST /containers/$_ID_/start

Content-Type: application/json

  

 {

  "Binds":\["/:/tmp:rw"\]

 }

  

**Now you have a running container where the /tmp directory maps back to the / of the host.** You have to login to the container to go wild on the host, but since you don't have direct access to said poor host, you need to have SSHd running on your container. The default Ubuntu image won't have SSHd up, so you'll have to use a different image (consider [baseimage-docker](http://phusion.github.io/baseimage-docker/)) or tweak around with the cmd - this very last part is left as an exercise for the reader... or scream at me enough in the comments and I'll come up with something. Alternatively, you could be able to use the copy endpoint ([documentation of the copy endpoint](https://docs.docker.com/reference/api/docker_remote_api_v1.14/#copy-files-or-folders-from-a-container)) but I've not experimented with that.

  

### Why this is worth writing about, or why I don't like un-authed admin interfaces

As I said at the beginning of the post, this writeup is mostly a time saver for those dealing with Docker during a penetration test. However, there is something that I don't like in the way Docker has gone about designing these APIs: the lack of any default auth entirely.

  

There are of course various reasons why you would ship such a critical feature with no authentication to be seen: time to market, the fact that after all it is disabled by default, or more likely the will to attain segregation of duties. Designing a secure authentication system is complicated, error prone and difficult, and ultimately not the role of an API. Delegating it is a better idea than botching it, so that it is clear where things go wrong.

  

I beg to disagree, and I'd have expected better from a company that produced [such a well thought discussion on the security issues with running SSH on a container](http://jpetazzo.github.io/2014/06/23/docker-ssh-considered-evil/). I'm going to argue that there is a **huge gap between having a clearly horrible authentication system and** **nothing at all.** The sad truth, as anyone who has done any pentest in his career will tell, is that the vast majority of the end users will run with whatever was shipped out of the box. Security people cheered when Oracle started to require users to set passwords during installation instead of setting default ones (ok ok, that's a long story), and the experience of the [Internet Census](http://internetcensus2012.bitbucket.org/paper.html) tells us how easy it is to forget to change that password, or to setup that auth.

  

Of course, security minded admins will set up things correctly and invest all the time needed to configure a secure environment. But what about the others? Was it so difficult to require a Basic Auth-powered password at startup?

  

Friction (as in, anything that makes your product harder to use) is bad, but pwned users are worse.

  
_Bonus: Here is a simple nmap probe for the Docker APIs._  
  

##############################NEXT PROBE##############################

\# Queries Docker APIs for the /version url containing version information.

#

Probe TCP docker q|GET /version HTTP/1.1\\r\\n\\r\\n|

rarity 7

ports 2375

sslports 2376

  

match docker m|.\*{"ApiVersion":"(.\*)","Arch".\*"GitCommit":"(.\*)","GoVersion".\*"Os":"(.\*)","Version":"(.\*)"}.\*| p/Docker remote API/ v/$1/ o/$3/ i/GitCommit:$2 DockerVersion:$4/

#### [Source](http://blog.nibblesec.org/feeds/7063128227393639643/comments/default)

<br/>
---
