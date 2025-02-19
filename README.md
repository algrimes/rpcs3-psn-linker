Keep your PS3 game library intact, compact, and hosted where you like. This tiny script installs your PS3 digital/hdd/psn games from an any path to RPCS3 without needing to move games or make copies into the emulators internal hard drive.

```
Usage:

rpcs3-psn-linker [/path/to/PSN/games/directory] [/path/to/rpcs3/root/folder] [/path/to/sfo]`

e.g.

./rpcs3-psn-linker [/media/games/ps3/digita] [~/Library/Application\ Support/rpcs3] [/home/to/sfo]`

```

There is an interactive prompt that warns about removing matching titleids from rpcs3s hdd, pipe `yes` to it to use it for automation. 


####Overview

One reason why RPCS3 asks you to install digital games into `/dev_hdd0/game` folder is nothing to do with the emulator itself - certain games were developed with fully-qualified-hard-coded paths to their assets that only work within a folder structure that exactly matches the PS3 filesystem. I saw this behaviour first hand when, after ignoring the quickstart guide, installed 2 PSN games from my collection. One worked, another didn't. The one that didnt was spitting errors into the console:

`E sys_fs: 'sys_fs_open' failed with 0x80010006 : CELL_ENOENT, “/dev_hdd0/game/NP blah blah`

This project gets round this limitation and creates symlinks within RPCS3's internal hdd to point to your already-hosted collection of folder-format PSN games, making these hard-coded games believe they are running in a PS3 fs, and avoiding the need to make copies of them in RPCS3's /dev_hdd0/game folder or have additional package installation steps.

The power of this is when you already have a collection of PSN folder games that you are hosting somewhere e.g. you are already serving digital games in folder format to your PS3 using ps3netsrv's GAMEI folder.

Combining these two approaches, you can now have a single hosted set of digital games that you can run both on your PS3 and RPCS3 seamlessly.

You could combine this script with `fswatch` and `launchctl` or `supervisord` to automatically add games to your RPCS3 library when they are added to your games folder.


####Dependencies

[SFO](https://github.com/hippie68/sfo/releases)

Required to extract the title_id from the PARAM.SFO so the correct folder structure can be created in RPCS3's virtual hard drive.

Pass the full path to where the executable is as the third argument.

####Required Directory structure

It assumes all games are installed at the same level underneath the directory you provide. Inside each folder, its expected to take the layout of a PS3 digital game. At minimum there will be a `USRDIR` directoru and a `PARAM.SFO` file.

```
/path/to/PSN/games/directory (the first argument you provide)
  > game 1
     > USRDIR
     > PARAM.SFO
  > game 2
     > USRDIR
     > PARAM.SFO
```
  

