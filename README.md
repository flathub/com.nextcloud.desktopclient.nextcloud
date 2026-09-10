# Flatpak Manifest for Nextcloud Desktop

## Build and run
Clone this repo (including submodules):
```
git clone --recurse-submodules https://github.com/flathub/com.nextcloud.desktopclient.nextcloud.git
```
And follow the [build & run instructions in the Flathub docs](https://docs.flathub.org/docs/for-app-authors/submission#build-and-install).

## Flatpak specific tweaks
### Enable GNOME Files / Nautilus Integration
Make sure you:
- Installed the [Nexcloud Desktop from Flathub](https://flathub.org/apps/details/com.nextcloud.desktopclient.nextcloud).
- Installed [GNOME Files / Nautilus](https://gitlab.gnome.org/GNOME/nautilus) and [nautilus-python](https://gitlab.gnome.org/GNOME/nautilus-python) from your distros package manager (e.g. `sudo apt install nautilus python3-nautilus` for Ubuntu 24.04).
Create a symlink to [Nextclouds Nautilus script](https://github.com/nextcloud/desktop/blob/a595d5bcb6f13f4ea4080b694682a5ae43e2f769/shell_integration/nautilus/syncstate.py) (which comes with the Flatpak):
```
nc=$(readlink -f "$(flatpak info -l com.nextcloud.desktopclient.nextcloud)/../../../")
syncstate='/nautilus-python/extensions/syncstate-Nextcloud.py'  # for Nautilus
#syncstate='/caja-python/extensions/syncstate-Nextcloud.py'  # for Caja
#syncstate='/nemo-python/extensions/syncstate-Nextcloud.py'  # for Nemo
nc_syncstate_path="${nc}/current/active/files/share${syncstate}"
user_syncstate_path="${XDG_DATA_HOME:-${HOME}/.local/share}${syncstate}"

mkdir -v -p "$(dirname ${user_syncstate_path})"
ln -v -s "${nc_syncstate_path}" "${user_syncstate_path}"
```
After you restart Nautilus, the script loads and enables the sync state icons as well as a right-click menu for your Nextcloud files in Nautilus.

When using the respective `syncstate=` line it might also work for Caja (MATEs file manager) or Nemo (Cinnamons file manager in Linux Mint). But this was only tested with Nautilus on Ubuntu 24.04 so your millage may vary.
