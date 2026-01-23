+++
title = "running docker on windows over ssh"
description = "ramblings about avoiding docker for mac and using windows from the command line"
date = 2021-03-26
updated = 2021-04-16
[extra]
weight = 3
[taxonomies]
tags=["work", "remote"]
+++

So I'm locked out of my apartment for about 7 weeks, with only a laptop and a
keyboard.

Here are some notes about my setup.

## docker 4 mac

There is a native ARM64 build, but it comes with an [extensive list of known
issues](https://docs.docker.com/docker-for-mac/apple-m1/#known-issues). In
particular, emulation of x86-based containers is sketchy and the disk
performance is [not very good](https://github.com/docker/for-mac/issues/1592)
when mounting host folders in the container.

Normally, I have access to a Windows (home) or Linux (office) machine and
both offer a better docker experience compared to MacOS. Since my apartment
will continue to have internet access, why run a gimped container setup on my
MacBook Air when I can ~~do something pointless~~ learn something new??

## my setup

I have a Windows machine and a Raspberry Pi at home. My plan? Use Wake-On-LAN
to wake my Windows machine, run docker on it for work and put it back to
sleep when done.

I found a nice little program called [wol](https://github.com/sabhiram/go-wol)
which let me save the MAC address of my Windows machine under an alias, which
made the wakeup script self-explanatory.

```bash
#!/usr/bin/env bash
ssh piathome '/home/pi/go/bin/wol wake winathome'
```

Next up was enabling Wake-On-LAN on my Windows machine, which turned out to be
quite simple. After a couple of wake-and-sleep cycles later I was confident
that this setup would actually work.

At this point I could wake the machine from sleep, and since I already had
enabled the built-in OpenSSH server, I searched online and found an eldritch
command to put it back to sleep. Behold:

```bash
#!/usr/bin/env bash
ssh winathome -t 'cmd /c \"rundll32.exe powrprof.dll,SetSuspendState 0,1,0\"'
```

Now it was time to automate these steps (as steps in a
[tmuxinator](https://github.com/tmuxinator/tmuxinator)
project) and move on to docker ~~shit~~!

Starting, stopping and all container interactions worked without any problem.
This is my script for port-forwarding ports and starting the backend services
in a work project:

```bash
#!/usr/bin/env bash
ssh winathome -L 3200:localhost:3200 -t 'cmd /c \"cd /dE:/work/xyz && docker-compose up\"'
```

and maybe an interactive session as well.

```bash
#!/usr/bin/env bash
ssh winathome -t 'cmd /k \"cd /dE:/work/xyz\"'
```

This actually works surprisingly well, now I mostly do:

- bring machine up: `tmuxinator start workday`
- work on project(s): `tmuxinator start/stop xyz`
- bring machine down: `tmuxinator stop workday`

## troubles

Of course, it was not exactly a smooth start:

- `git` failed due to something related to a credential manager - "fixed" by switching remote from https to ssh
- azure container registry at $DAYJOB need a ~daily login `az acr login` which also failed due to similar error - resolved after [reading the fucking manual](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentication)
and [learning about docker credentials](https://www.projectatomic.io/blog/2016/03/docker-credentials-store/)

```cmd
@rem remove "credsStore" key from config which does not work with ssh session and seems to be default on desktop
jq "del(.credsStore)" %userprofile%\.docker\config.json | sponge %userprofile%\.docker\config.json

az acr login --name xyz --expose-token | jq -r .accessToken > .aztoken
cat .aztoken | docker login xyz.azurecr.io -u 00000000-0000-0000-0000-000000000000 --password-stdin
del .aztoken
```
