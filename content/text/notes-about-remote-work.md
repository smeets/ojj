+++
title = "notes about remote work"
description = "ramblings of a madman about avoiding docker for mac and using windows from the command line"
date = 2021-03-26
updated = 2021-03-29
[taxonomies]
tags=["work", "docker", "remote"]
+++
	
So I'm locked out of my apartment for about 7 weeks, with only a laptop and a
keyboard. 

Here are some notes about my setup.

## docker 4 mac

TLDR; probably don't do it.

There is a native ARM64 build, but it comes with an [extensive list of known
issues](https://docs.docker.com/docker-for-mac/apple-m1/#known-issues). In
addition, the disk performance is [not very
good](https://github.com/docker/for-mac/issues/1592) when mounting host
folders in the container.

Somehow my apartment will keep its internet during the renovation, so why take
the big desktop with me when I can learn some new stuff by leaving it home...
;]

## my setup

Essentially, I have a Windows machine and a Raspberry Pi. My plan? Use Wake-On-LAN
to wake my Windows machine, run docker on it for work and put it back to sleep when done.

**1.** Expose a Raspberry Pi (running 24/7) via [Tailscale](https://tailscale.com/) and setup 
[wol](https://github.com/sabhiram/go-wol) with MAC address of my Windows machine

**2.** Enable the OpenSSH server and Wake-On-LAN on my docker machine (Windows) 

**3.** SSH into RPI and send WOL packet to Windows machine
```bash
#!/usr/bin/env bash
ssh homepi '/home/pi/go/bin/wol wake dockerhost'
```

**4.** SSH into Windows machine and start my docker stuff

```bash
#!/usr/bin/env bash
ssh dockerhost -L 3200:localhost:3200 -t 'cmd /c \"cd /dE:/work/xyz && docker-compose up\"'
```

and maybe an interactive session as well.

```bash
#!/usr/bin/env bash
ssh dockerhost -t 'cmd /k \"cd /dE:/work/xyz\"'
```

**5.** When I'm done for the day I can put it back to sleep

```bash
#!/usr/bin/env bash
ssh dockerhost -t 'cmd /c \"rundll32.exe powrprof.dll,SetSuspendState 0,1,0\"'
```

This actually works surprisingly well, especially combined with e.g.
[tmuxinator](https://github.com/tmuxinator/tmuxinator) for  extra automation:

- bring machine up: `tmuxinator start workday` 
- work on project(s): `tmuxinator start/stop some-project` 
- bring machine down: `tmuxinator stop workday`

Looks like I'll need to keep an eye out on [innernet](https://blog.tonari.no/introducing-innernet)
and see if that could replace my reliance on Tailscale. It requires a coordinator though (which
basically is my reason for using Tailscale, outsourcing and all that), but that could be worked around
with a dynamically updated DNS config. Maybe [Cloudflare has an API?](https://api.cloudflare.com/#dns-records-for-a-zone-update-dns-record)

## docker 4 windows login errors

Of course, it was not exactly a smooth start:

- `git` failed due to something related to a credential manager - "fixed" by switching remote from https to ssh
- `az acr login` also failed due to similar error - resolved after [reading the fucking manual](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentication)
and [learning about docker credentials](https://www.projectatomic.io/blog/2016/03/docker-credentials-store/)

```cmd
@rem remove "credsStore" key from config (seems to default to desktop, which does not work with ssh session)
jq "del(.credsStore)" %userprofile%\.docker\config.json | sponge %userprofile%\.docker\config.json

@REM output --raw string field (without quotes)
az acr login --name xyz --expose-token | jq -r .accessToken > .aztoken
cat .aztoken | docker login xyz.azurecr.io -u 00000000-0000-0000-0000-000000000000 --password-stdin
del .aztoken
```

 