# Davinchi-to-PopOS-installer

A minimal fork of **JDInstaller** focused on installing **DaVinci Resolve** on **Pop!_OS 24.04** in a few commands.

This project is based on the DaVinci Resolve role from JDInstaller and on the workflow described in the Reddit post:
https://www.reddit.com/r/Ubuntu/comments/1g8z67w/i_wrote_a_script_to_install_davinci_resolve_on/

Credit and upstream:
- JDInstaller by leinardi: https://github.com/leinardi/JDInstaller

## What this fork changes

- Adds missing dependencies (including `libglu1-mesa`, `patchelf`, `rsync`).
- Ensures `/opt/davinci-resolve/Apple Immersive` is writable by the `davinci` group to avoid
  "Failed to create application support directories" on first launch.
- Keeps the original JDInstaller approach: extract the AppImage, replace outdated bundled libs
  with system libraries, set udev rules, desktop files, and GPU OpenCL packages.

## Install (Pop!_OS 24.04)

1) Install requirements:
```bash
sudo apt update
sudo apt install -y git make
```

2) Clone this repo:
```bash
git clone https://github.com/Entoniel/Davinchi-to-PopOS-installer.git
cd Davinchi-to-PopOS-installer
```

3) (Optional) update the Blackmagic download form data:
Edit `group_vars/all.yml` and set `davinci_resolve.email`, `phone`, `country`, etc.

4) Run the installer:
```bash
make install TAGS=davinci_resolve
```

5) Log out and log back in (required for the `davinci` group).

6) Start Resolve:
```bash
/opt/davinci-resolve/bin/resolve
```

## Use an already downloaded ZIP (optional)

If you already have the official ZIP file, use:
```bash
make install TAGS=davinci_resolve \
  EXTRA_VARS="davinci_resolve_zip_file=/path/to/DaVinci_Resolve_20.x_Linux.zip"
```

## Studio version

To install Studio, set in `group_vars/all.yml`:
```yaml
davinci_resolve:
  install_studio: true
```

## Notes

- This installer downloads the official ZIP from Blackmagic or uses a local ZIP.
- It installs into `/opt/davinci-resolve` and configures desktop files and udev rules.
- Tested on Pop!_OS 24.04. If you find missing dependencies for specific workflows, open an issue.
