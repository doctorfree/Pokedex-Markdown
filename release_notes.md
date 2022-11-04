# Release Notes

## Table of contents

1. [Overview](#overview)
1. [Installation](#installation)
1. [Configuration](#configuration)
1. [Usage](#usage)
1. [Removal](#removal)
1. [Support](#support)
1. [Changelog](#changelog)

## Overview

The Pokédex (Pokémon Encyclopedia) is an electronic device designed to catalog and provide information regarding the various species of Pokémon. The name Pokédex is a portmanteau of Pokémon and index. In the video games, whenever a Pokémon is first caught, its height, weight, species type, and a short description will be added to a player's Pokédex. Each region has its own Pokédex, which differ in appearance, species of Pokémon catalogued, and functions. In *Pokémon Legends: Arceus*, which takes place long before any other Pokémon games, players are tasked with assembling the first ever Pokédex.

This repository contains a Pokémon Pokédex in markdown format. It was generated from [Veekun's Pokédex](https://github.com/veekun/pokedex) by converting the CSV format Pokédex to Markdown using a script I wrote for [Asciiville](https://github.com/doctorfree/Asciiville.git).

These are the release notes for Version 1.0.2 Release 1 of the Markdown Markdown Pokédex.

## Installation

The Markdown Pokédex can be installed on Windows, Mac, or Linux. The following installation instructions are for Mac and Linux. Windows users can follow similar steps.

### Download the release archive

[Download the latest release](https://github.com/doctorfree/Pokedex-Markdown/releases/latest).

Those familiar with `wget` can download this release from the command line with:

```shell
wget --quiet -O ~/Downloads/Pokedex-Markdown-v1.0.2r1.tar.gz \
  https://github.com/doctorfree/Pokedex-Markdown/archive/refs/tags/v1.0.2r1.tar.gz
```

### Extract the release archive

Currently release archives are available in either ZIP or compressed tar archive format.

To extract the ZIP archive:

```shell
cd /path/to/your/vaults # e.g. `cd ~/Documents`
unzip /path/to/Pokedex-Markdown-1.0.2r1.zip
```

To extract the compressed tar archive:

```shell
cd /path/to/your/vaults # e.g. `cd ~/Documents`
tar xf /path/to/Pokedex-Markdown-1.0.2r1.tar.gz
```

Once extracted, the Markdown Pokédex vault is now available in `/path/to/your/vaults/Pokedex-Markdown-1.0.2r1/`.

The downloaded archive can be deleted:

```shell
rm -f /path/to/Pokedex-Markdown-1.0.2r1.zip
```

or

```shell
rm -f /path/to/Pokedex-Markdown-1.0.2r1.tar.gz
```

## Configuration

The Markdown Pokédex is pre-configured for use with [Obsidian](https://obsidian.md). Install Obsidian for your platform by clicking the appropriate installation link at the Obsidian website. Obsidian is available for Windows, Mac, and Linux as well as mobile devices.

Add a new vault in Obsidian with `Open folder as vault` and navigate to the `Pokedex-Markdown-1.0.2r1` extracted folder. When prompted, `Trust` and enable the `Dataview` plugin if it is not already enabled.

Obsidian is required for some features but is not necessary to view the Markdown Pokédex. Any markdown viewer/editor can be used. If the Markdown Pokédex is extracted into a website folder, it can be viewed using most browsers.

## Usage

This repository is organized as an Obsidian vault containing the Pokédex in markdown format. It can be viewed using any markdown viewer but if Obsidian is used then many additional features will be available including queries using the [Dataview](https://blacksmithgu.github.io/obsidian-dataview/) plugin for [Obsidian](https://obsidian.md/).

### **For the optimal experience, open this vault in Obsidian!**

1. [Download the vault](https://github.com/doctorfree/Pokedex-Markdown/releases/latest)
3. Open the vault in Obsidian via "Open another vault -> Open folder as vault"
4. Trust us. :) 
5. When Obsidian opens the settings, hit the switch on "Dataview" to enable the plugin
6. Done! The Markdown Pokédex vault is now available to you in its purest and most useful form!

### Pokédex Queries

The Markdown Pokédex has been curated with metadata allowing queries to be performed using the Obsidian Dataview plugin. Sample queries along with the code used to perform them can be viewed in the [Pokédex Queries](pokedex_queries.md) document.

## Removal

To remove the Markdown Pokédex simply remove the extracted folder and its contents:

```shell
cd /path/to/your/vaults # e.g. `cd ~/Documents`
rm -rf Pokedex-Markdown-1.0.2r1
```

## Support

Support the development and improvement of the Markdown Pokédex by [sponsoring the Projects of Doctorfree](https://github.com/sponsors/doctorfree).

<a href="https://www.buymeacoffee.com/doctorfree"><img src="https://img.buymeacoffee.com/button-api/?text=Buy me a coffee&emoji=&slug=doctorfree&button_colour=5F7FFF&font_colour=ffffff&font_family=Lato&outline_colour=000000&coffee_colour=FFDD00"></a>

## Changelog

View the full changelog for this release at https://github.com/doctorfree/Pokedex-Markdown/blob/v1.0.2r1/CHANGELOG.md
