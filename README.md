# Manual ttfautohint Instruction Set Adjustments

## About

This document describes an approach to view and manually adjust the automated [`ttfautohint`](https://www.freetype.org/ttfautohint/) bytecode instruction sets in TrueType font binaries with delta exceptions.

## Audience

Typeface developers who need an iterative workflow to view and modify the automatic bytecode instruction sets that are defined during execution of the `ttfautohint` executable on TrueType font binaries (`*.ttf` files).

## Terminology

- **Control instructions files** (CIF) - `ttfautohint` scripts that define delta exceptions
- **Delta exceptions** - type size-specific x-axis and y-axis adjustment definitions for individual (or sequential ranges of) points in glyph outlines

## Background

[`ttfautohint`](https://www.freetype.org/ttfautohint/doc/ttfautohint.htm) is a free software tool that automates script-specific [feature analysis](https://www.freetype.org/ttfautohint/doc/ttfautohint.html#feature-analysis), [computation of appropriate blue zones](https://www.freetype.org/ttfautohint/doc/ttfautohint.html#blue-zones), and [definition of type-size specific optimal hint sets](https://www.freetype.org/ttfautohint/doc/ttfautohint.html#hint-sets) in TrueType fonts (`*.ttf`) using data from the FreeType auto-hinting module.  Instruction sets are defined on the horizontal axis only leading to vertical-only hinting.  A deeper dive into the technical details of this process is available in the [ttfautohint Background and Technical Details](https://www.freetype.org/ttfautohint/doc/ttfautohint.html#background-and-technical-details) section of the documentation.

`ttfautohint` supports manual modification of the instructed shapes at given type sizes through the use of [delta exception](https://www.freetype.org/ttfautohint/doc/ttfautohint.html#delta-exceptions) definitions that are stored in [control instructions files](https://www.freetype.org/ttfautohint/doc/ttfautohint.html#control-instructions).  The type developer can manually master fonts with x- and y-axis instruction set modifications of outline point positions at a granularity of 1/8 pixel dimension and define points in the shape that should not be interpolated. These modifications lead to appropriately adjusted calculations of all interpolated "untouched" points in the outline between the "touched" points as defined by the automated ttfautohint algorithm and modified definitions in delta exceptions that are specified by the type developer.

This document describes an approach to review glyph outlines at any desired pixel per em size (ppem = point size when viewed at 72 dpi), identify glyph-specific outline point numbers for use in the definition of delta exceptions, and apply the delta exceptions in control instructions files with the `ttfautohint` tool.  The approach is an iterative mastering process that allows the developer to review size-specific shapes, modify instruction sets where necessary, re-compile + re-hint fonts, and then view the shapes with modified instruction sets.


## Installation of Tools

The approach described here requires the following free software tools:

- FreeType 2 library ([Source](https://git.savannah.gnu.org/cgit/freetype/freetype2.git/))
- `ttfautothint` executable application ([Source](https://git.savannah.gnu.org/cgit/freetype/freetype2-demos.git/))
- `ft-view` freetype2-demo tool - Source Foundry customized branch ([Source](https://github.com/source-foundry/freetype2-demos))


### Compile FreeType 2 library

The following is a generic approach to compilation of the FreeType 2 library for use in this workflow.  For more detailed instructions, including necessary dependencies on your platform, please refer to the [FreeType project documentation](https://www.freetype.org/download.html).

```
$ curl -L -O https://download.savannah.gnu.org/releases/freetype/freetype-2.9.1.tar.gz
$ tar -xvzf freetype-2.9.1.tar.gz && rm freetype-2.9.1.tar.gz
$ mv freetype-2.9.1 freetype2
$ cd freetype2
$ ./configure && make
$ cd ..
```


### Compile `ft-view` executable

This approach uses the [Source Foundry branch of the freetype2-demos tools](https://github.com/source-foundry/freetype2-demos) (cs-custom branch) that includes source modifications to improve support of this workflow.  The upstream project source is available at https://git.savannah.gnu.org/cgit/freetype/freetype2-demos.git/ if you prefer to use the default tool builds as released by the FreeType project. Just swap the URL for the source in the `git clone` step and proceed with the rest of the compile instructions.

**Important**: *Pull the freetype-demos directory to the root directory that contains the `freetype2` directory created in the FreeType 2 library compile steps above.  The directory renaming to `freetype2` in the instructions above is mandatory to compile the freetype-demo tools that contain `ft-view`*.

```
$ git clone https://github.com/source-foundry/freetype2-demos.git
$ cd freetype2-demos
$ make
```

### Install ttfautohint

Install `ttfautohint` using [the platform-specific instructions in the ttfautohint documentation](https://www.freetype.org/ttfautohint/doc/ttfautohint.html#compilation-and-installation) or the [ttfautohint-build project README](https://github.com/source-foundry/ttfautohint-build).


