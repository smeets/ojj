+++
title = "notes about remote work"
date = 2021-03-26
updated = 2021-03-26
+++

# notes about remote work

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

Somehow my apartment will still have internet, so why take the big desktop
with me when I can learn some new stuff by leaving it home... ;]

- Expose a Raspberry Pi (running 24/7) via [Tailscale](https://tailscale.com/)
- Enable the OpenSSH server and Wake-On-LAN on my docker machine (Windows) 
- SSH into the RPI and send WOL packet from there (sending WOL across LANs is tricky) 
- SSH (with port-forward) into windows and launch `docker-compose ... up` directly (for automatic cleanup) 
- When I'm done for the day I can put it back to sleep

This actually works surprisingly well, especially combined with e.g.
[tmuxinator](https://github.com/tmuxinator/tmuxinator) for  extra automation:

- bring machine up: `tmuxinator start workday` 
- work on project(s): `tmuxinator start/stop some-project` 
- bring machine down: `tmuxinator stop workday`

## docker 4 windows login errors

- `git` failing due to something related to a credential manager - fixed by switching remote from https to ssh
- `az acr login` failing due to similar error - resolved after [reading the fucking manual](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentication)
and [learning about docker credentials](https://www.projectatomic.io/blog/2016/03/docker-credentials-store/)

```bash
# if credStore == desktop, fucking delete it
sed -i '/credStore": "desktop/d' ~/.docker/config.json
set TOKEN=$(az acr login xyz --expose-token | jq .accessToken)
docker login xyz.azurecr.io -u 0000-0000... -p ${TOKEN}
```
