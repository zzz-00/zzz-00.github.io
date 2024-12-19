---
title: "ArchWSL on Windows"
date: 2024-12-18
categories: [Linux, Arch Linux]
tags: [WSL2, Arch Linux]
description: Configure Archwsl on Windows
---

For convenience, I recommend using scoop to install ArchWSL directly.

# Installing Scoop

## Modify installing directory

Before installing scoop, we need to modify the default installing directory.

Default directory:

- user: C:\Users\<user\>\scoop
- global: C:\ProgramData\scoop

Modify sample:

```powershell
$env:SCOOP='D:\UserScoop'
[Environment]::SetEnvironmentVariable('USERSCOOP', $env:SCOOP, 'User')

$env:SCOOP_GLOBAL='D:\GlobalScoop'
[Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $env:SCOOP_GLOBAL, 'Machine')
```

You need to create the above two folders before modifying installing directory.

## Installing

```powershell
Set-ExecutionPolicy ByPass -Scope Process -Force
iex "& {$(irm get.scoop.sh)} -RunAsAdmin"
```

Update `scoop` after installing it.

```powershell
scoop update
```

## Optional

### Get the NerdFonts

We first need to add the `nerd-fonts` bucket.

```powershell
scoop bucket add nerd-fonts
```

Next install the font packages

```powershell
scoop install Meslo-NF Meslo-NF-Mono Hack-NF Hack-NF-Mono FiraCode-NF FiraCode-NF-Mono FiraMono-NF FiraMono-NF-Mono
```

Then you should set your Windows terminal to use one of the fonts you installed by going to the settings.

More information about `nerd-fonts` can be found [here](https://www.nerdfonts.com/).

## Installing ArchWSL

We need to get ArchWSL from the `extra` bucket and set the version of WSL to version 2 which is recommended.

```powershell
wsl --set-version 2
scoop bucker add extra
scoop install archwsl
```

Or you can run the following command before install ArchWSL.

```powershell
wsl --install --no-distribution
```

> NOTE: Run Windows terminal as administrator.

## Configuring and setting up ArchWSL

Start by running the `arch.exe` program.

```powershell
arch.exe
```

Configuring and setting up ArchWSL is the same as configuring normal Arch Linux, so I won’t go into details here. For details, please visit [this page](https://zzz-00.github.io/posts/Arch-Linux-Configuration/).

After configuring users in ArchWSl, we need to return to Windows terminal to set the default user.

```powershell
arch.exe config --default-user <user_name>
```