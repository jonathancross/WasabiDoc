---
{
  "title": "Deterministic Build",
  "description": "A step by step guide on how to reproduce the deterministic builds of Wasabi. This is the Wasabi documentation, an archive of knowledge about the open-source, non-custodial and privacy-focused Bitcoin wallet for desktop."
}
---

# Deterministic Build

[[toc]]

---

> Reproducible [or deterministic] builds are a set of software development practices that create an independently-verifiable path from source to binary code.  ~ [ReproducibleBuilds.org](https://reproducible-builds.org/)

This guide describes how to reproduce Wasabi's builds.

---

## 1. Assert Correct Environment

In order to reproduce Wasabi's builds you need [Git](https://git-scm.com/downloads), [Windows 10](https://www.microsoft.com/en-us/software-download/windows10ISO) and the version of [.NET Core SDK](https://www.microsoft.com/net/download) that was the most recent in the time of building the [release](https://github.com/zkSNACKs/WalletWasabi/releases).

## 2. Reproduce Builds

```sh
git clone https://github.com/zkSNACKs/WalletWasabi.git
git checkout {hash of the release} // This works from 1.1.3 release, https://github.com/zkSNACKs/WalletWasabi/releases
cd WalletWasabi/WalletWasabi.Packager/
dotnet restore
dotnet build
dotnet run -- --onlybinaries
```

This will build our binaries for Windows, OSX and Linux from source code and open them in a file explorer for you.

![](https://i.imgur.com/8XAQzz4.png)

## 3. Verify Builds

You can compare our binaries with the downloads we have on the website: [https://wasabiwallet.io/](https://wasabiwallet.io/)
In order to end-to-end verify all the downloaded packages you need a Windows, a Linux, and an OSX machine.

![](https://i.imgur.com/aI9Kx0c.png)

### Windows

After you installed Wasabi from the `.msi`, it will be in `C:\Program Files\WasabiWallet` folder.
You can compare it with your build:

```sh
git diff --no-index win7-x64 "C:\Program Files\WasabiWallet"
```

### Linux && OSX

You can use the Windows Subsystem for Linux to verify all the packages in one go.
At the time of writing this guide we provide a `.tar.gz` and a `.deb` package for Linux and .dmg for OSX.
Install the `.deb` package and extract the `tar.gz` and `.dmg` packages, then compare them with your build.

After installing WSL, just type `wsl` in explorer where your downloaded and built packages are located.

![](https://i.imgur.com/yRUjxvG.png)

#### .deb

```sh
sudo dpkg -i Wasabi-${currentVersion}.deb
git diff --no-index linux-x64/ /usr/local/bin/wasabiwallet/
```

#### .tar.gz

```sh
tar -pxzf WasabiLinux-${currentVersion}.tar.gz
git diff --no-index linux-x64/ WasabiLinux-${currentVersion}
```

#### .dmg

You will need to install [7z](https://www.7-zip.org/) (or something else) to extract the `.dmg`: `sudo apt install p7zip-full`

```sh
7z x Wasabi-${currentVersion}.dmg -oWasabiOsx
git diff --no-index osx-x64/ WasabiOsx/Wasabi\ Wallet.App/Contents/MacOS/
```
