# Installation guide to run Penpot in Podman
This is an unofficial guide and may not be up to date forever. \
I am affiliated with neither the [Kaleidos](https://www.kaleidos.net/) team nor the [Penpot](https://penpot.app/) project. \
Updated 2026-07-03.

Inspired from [Mairin Duffy](https://blog.linuxgrrl.com/)'s 2022
[blog](https://blog.linuxgrrl.com/2022/01/19/running-penpot-locally-docker-free-with-podman/).

[![Alt Text](https://site-assets.plasmic.app/2908dc70513b511aa49701f6c7ef0644.svg)](https://penpot.app/)

### MacOS and Windows (Podman installation)
Follow the official [Podman installation guide](https://podman.io/docs/installation).

Then, \
`podman machine init` \
`podman machine start` \
`podman info`

`podman machine ssh`, edit `sudo nano /etc/containers/registries.conf` with your editor of choice, add `unqualified-search-registries = ["docker.io"]` and save. \
`exit` the SSH shell, and the rest is like on Linux except for cloning the repo.

Be careful to clone inside your $HOME directory (on the same drive podman is installed to), \
otherwise the underlying Linux VM won't find the files for podman to build.

Cockpit and cockpit-podman are Linux-only. Skip them on Mac/Windows.

### Linux (full example with Arch)
`sudo pacman -S podman cockpit cockpit-podman podman-compose podman-docker` # podman-plugins is not available in pacman repos.

> Only necessary for Arch, and Debian derivatives. Fedora doesn't need it. \
>	`sudo vim /etc/containers/registries.conf` \
> [Insert] \
>	`unqualified-search-registries = ["docker.io"]` \
>	[Esc] :wq

`git clone https://github.com/penpot/penpot.git` \
`cd ~/penpot/docker/images`

Shut down the container: \
`podman-compose -p penpot -f docker-compose.yaml down` \
(Re)start the container: \
`podman-compose -p penpot -f docker-compose.yaml up -d`

Create the user for Penpot: \
`podman exec -ti penpot_penpot-backend_1 python3 manage.py create-profile` \
It's worth checking `podman ps` if the name doesn't match. For example it could be `penpot-penpot-backend-1` with dashes.

`librewolf "http://localhost:9001"` # assuming you want to open the page in [LibreWolf](https://codeberg.org/librewolf). \
Tailor this last command to your needs, or directly type the URL in whatever browser you use.

### Subsequent executions
Once you've completed your first run and rebooted your device, you'll need to follow only these steps to use Penpot again: \
`podman-compose -p penpot -f docker-compose.yaml up -d` # starts the podman container. \
`librewolf "http://localhost:9001"` # brings you to the localhost page in your browser. \
Feel free to set aliases, add them to scripts or launchers.

**Enjoy!**
