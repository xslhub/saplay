# saplay

A Simple AutoPLAY shell script with minimal external dependencies. Only a POSIX compatible shell like `bash` or `dash` and a media player of your choice like `mpv` are required.

## Usage

### Watching all episodes of your favorite show

Press `q` to skip an episode or `ctrl+c` to stop playback.

```
$ cd shows/favorite
$ saplay
```

### Finding out how many episodes you already watched

```
$ saplay -h
01  /home/you/shows/favorite/e01/e01.mkv
02  /home/you/shows/favorite/e02/e02.mkv
03  /home/you/shows/favorite/e03/e03.mkv
```

### Skipping the first three episodes of your favorite show

```
$ saplay -s 3
```

### Skipping the first six episodes and using a different audio sink

```
$ saplay -os 6
```

### Append additional arguments to the video player to display no subtitles

```
$ saplay -a --no-sub
```

## Documentation

#### Scanning order

Unless changed in the config file, `saplay` will first scan for media files structured in the following way:
```
|- episode01
   |- main.mkv
   |- ...
|- episode02
   |- main.mkv
   |- :
|- :
```
In the example above, `saplay` will play `episode01/main.mkv` and then `episode02/main.mkv`. If no media files were found using the directory scan, the file scan is used, which scans for files structured like this:
```
|- episode01.mkv
|- episode02.mkv
```
The order of these scans can be switched using the `play_mode` setting in the config file or using the `-d` or `-f` command line options.

#### All command line options

```
Usage:
   saplay [options]
Options:
   -a <args>    Append additional arguments to player
   -d <path>    Scan for directories
   -f <path>    Scan for files
   -h <path>    Get history of played files in <path>
   --help       Display usage message
   -o           Use optional parameters set in the config file
   -s <n>       Skip <n> media files
   --version    Print version information
Press <ctrl+c> to stop. Press <q> to skip.
Without a path given, the working directory is used as path.
Without any parameters saplay will try -d and -f.
```

## Installation

Just copy `saplay` to any directory in your `$PATH`.
It can be in `/usr/local/bin`, `$HOME/.local/bin` or any other directory you want to use.
```
# Use any directory in your $PATH you want, instead of /usr/local/bin
chmod +x saplay
cp saplay /usr/local/bin
mkdir -p ~/.config/saplay
cp config ~/.config/saplay
```

## Configuration

In addition to the default scan mode, the media player, the arguments to the media player,
the optional arguments set when using `-o` as well as the regular expression used to find media files,
can be changed in the config file `~/.config/saplay/config`

```
# Default play mode, if none is selected
PLAY_MODE_DIRS="directories" # -d
PLAY_MODE_FILES="files" # -f
play_mode="$PLAY_MODE_DIRS"
# The movie player to use
player="mpv"
# Default arguments to use with player
args="-fs"
# Additional arguments supplied to player when using -o
args_opt="--audio-device='pulse/alsa_output.pci-0000_03_00.1.hdmi-stereo'"
# Regex used to find media files
regex='.*\.\(mkv\|avi\|mpeg\|mpg\|mp4\)'
```

## Dependencies

A POSIX compatible shell like `bash` or `dash` and a media player of your choice like `mpv`.

## Files and directories

- Config file: `$HOME/.config/saplay/config`
- History file: `$HOME/.local/state/saplay/history`
