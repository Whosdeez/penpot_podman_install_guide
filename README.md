# Installation guide to run Penpot in Podman
Inspired from [Mairin Duffy](https://blog.linuxgrrl.com/)'s 2022
[blog](https://blog.linuxgrrl.com/2022/01/19/running-penpot-locally-docker-free-with-podman/). \
Updated 2026-07-03

### MacOS and Windows (Podman installation)
Follow the official [Podman installation guide](https://podman.io/docs/installation).

Then, \
`podman machine init` \
`podman machine start` \
`podman info` \
`podman machine ssh`, edit `sudo nano /etc/containers/registries.conf` with your editor of choice. \
Add `unqualified-search-registries = ["docker.io"]` and save. \
`exit` the SSH shell, and the rest is like on Linux except for cloning the repo: \
Be careful to clone inside your $HOME directory (on the same drive podman is installed to), otherwise the underlying Linux VM won't find the files for podman to build.

### Linux (Arch full example)
`sudo pacman -S podman cockpit cockpit-podman podman-compose podman-docker` # podman-plugins not available in pacman.

Cockpit/cockpit-podman are Linux-only. Skip them on Mac/Windows.

> Only necessary for Arch, and Debian derivatives. Fedora doesn't need it. \
>	`sudo nvim /etc/containers/registries.conf` \
>   [Insert]
>	`unqualified-search-registries = ["docker.io"]` \
>	[Esc] :wq

`podman-compose -p penpot -f docker-compose.yaml down` \
`podman-compose -p penpot -f docker-compose.yaml up -d`

`git clone https://github.com/penpot/penpot.git` \
`cd ~/penpot/docker/images`

`podman exec -ti penpot_penpot-backend_1 python3 manage.py create-profile` \
It's worth checking `podman ps` if the name doesn't match. For example it could be `penpot-penpot-backend-1` with dashes.

`librewolf "http://localhost:9001"` # or whatever your browser is.
