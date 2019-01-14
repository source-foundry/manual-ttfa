# Manual ttfautohint Instruction Set Adjustments

## About

This document describes an approach to view and manually adjust the automated [TTFAutohint](https://www.freetype.org/ttfautohint/) bytecode instruction sets in TrueType font binaries with delta exceptions.


## Installation

The approach described here requires the following free software tools:

- FreeType 2 library ([Source](https://git.savannah.gnu.org/cgit/freetype/freetype2.git/))
- `ttfautothint` executable application ([Source](https://git.savannah.gnu.org/cgit/freetype/freetype2-demos.git/))
- `ft-view` freetype2-demo tool ([Source](https://git.savannah.gnu.org/cgit/freetype/freetype2-demos.git/))


### Compile the FreeType 2 library

```
$ curl -L -O https://download.savannah.gnu.org/releases/freetype/freetype-2.9.1.tar.gz
$ tar -xvzf freetype-2.9.1.tar.gz
$ mv freetype-2.9.1 freetype2
$ cd freetype2
$ ./configure && make
$ cd ..
```


### Compile `ft-view` executable

**Important**: *The freetype-demos directory should be pulled to the root directory that contains the `freetype2` directory created in the FreeType 2 library compile steps above.  The directory renaming to `freetype2` demonstrated in the steps above is mandatory to compile the freetype-demo tools that contain `ft-view`.*

```
$ git clone https://git.savannah.gnu.org/git/freetype/freetype2-demos.git
$ cd freetype2-demos
$ make
```

### Install ttfautohint

Install `ttfautohint` using [the instructions in the ttfautohint documentation](https://www.freetype.org/ttfautohint/doc/ttfautohint.html#compilation-and-installation).


