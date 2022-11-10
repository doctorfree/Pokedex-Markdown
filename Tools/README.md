# Tools

This is a small collection of scripts used to assist the migration of the original CSV documents to Markdown, add metadata, and fix areas where the documents were missing data.

Mostly these scripts were used once and are no longer that useful. However, they serve well as examples of how to batch manipulate Markdown documents. Currently they are primarily reference material and likely only useful to those wishing to make mass changes to the Markdown documents.

Some of what was done with these scripts is now better implemented using the Obsidian Templater plugin. I keep these around so I can copy the relevant sections in future scripts. You can safely ignore these.

Scripts include the following:

- [Pokemon_fetch](#pokemon_fetch)
- [CSV_to_Markdown](#csv_to_markdown)
- [Splitting_large_markdown](#splitting_large_markdown)
- [add-link](add-link.md)
- [addtags](addtags.md)
- [append-also](append-also.md)
- [append-info](append-info.md)
- [awk-fixup-img-links](awk-fixup-img-links.md)
- [fixmeta](fixmeta.md)
- [head](head.md)
- [insert-template](insert-template.md)
- [insert-template2](insert-template2.md)
- [install-pandoc-filters](install-pandoc-filters.md)
- [mkmd](mkmd.md)
- [pre](pre.md)
- [prepend-name](prepend-name.md)
- [remthis](remthis.md)
- [sp](sp.md)
- [tag](tag.md)
- [tagmultiple](tagmultiple.md)
- [template.md](template.md)

## Pokemon_fetch

The `pokefetch` command retrieves and displays a Pokemon using the `pokeapi`. Pokemon to be retrieved can be specified by name or id.

```shell
#!/bin/bash

# Prepare ----------------------------------------------------------------------

have_gum=$(type -p gum)
[ "${have_gum}" ] || exit 1

usage() {
    printf "\nUsage: pokefetch [-aA] [-i id] [-n name] [-u]"
	printf "\nWhere:"
	printf "\n\t-a indicates no art"
	printf "\n\t-A indicates do not use ANSI code sequences (e.g. for colors)"
	printf "\n\t\t-A implies -a since retrieved art includes ANSI code sequences"
	printf "\n\t-i 'id' specifies the Pokemon ID to retrieve"
	printf "\n\t-n 'name' specifies the Pokemon name to retrieve"
	printf "\n\t-u displays this usage message and exits"
	printf "\nDefault: random Pokemon ID"
	printf "\nExample: pokefetch -n Genesect\n"
	exit 1
}

pokemon=
fetchart=1
use_ansi=1
while getopts "aAi:n:u" flag; do
    case $flag in
        a)
            fetchart=
            ;;
        A)
            fetchart=
            use_ansi=
            ;;
        i)
            pokemon="$OPTARG"
			[ ${pokemon} -lt 1 ] || [ ${pokemon} -gt 905 ] && pokemon= 
            ;;
        n)
            pokemon="$OPTARG"
            pokemon=`echo "${pokemon}" | tr '[:upper:]' '[:lower:]'`
            ;;
        u)
            usage
            ;;
    esac
done
shift $(( OPTIND - 1 ))

[ "${pokemon}" ] || pokemon=${1:-$(shuf -i 1-905 -n 1)}

data_pokemon=$(curl -fsLS "https://pokeapi.co/api/v2/pokemon/${pokemon}")
data_species=$(curl -fsLS "$(echo "${data_pokemon}" | jq --raw-output .species.url)")

id=$(echo "${data_species}" | jq --raw-output .id)
name=$(echo "${data_species}" | jq --raw-output '.names | .[] | select(.language.name == "en").name')
category=$(echo "${data_species}" | jq --raw-output '.genera | .[] | select(.language.name == "en").genus')
if [ "${use_ansi}" ]
then
  title="▐[1;7m No.$(printf '%03d' "${id}") [0m▌ [1m${name} - ${category}[0m"
else
  title=" Number $(printf '%03d' "${id}") ${name} - ${category}"
fi

for type in $(echo "${data_pokemon}" | jq --raw-output '.types | .[].type.name' | tr '[:lower:]' '[:upper:]'); do
	case "${type}" in
		NORMAL)   color=7  ;;
		FIRE)     color=9  ;;
		WATER)    color=12 ;;
		ELECTRIC) color=11 ;;
		GRASS)    color=10 ;;
		ICE)      color=14 ;;
		FIGHTING) color=1  ;;
		POISON)   color=5  ;;
		GROUND)   color=11 ;;
		FLYING)   color=6  ;;
		PSYCHIC)  color=13 ;;
		BUG)      color=2  ;;
		ROCK)     color=3  ;;
		GHOST)    color=4  ;;
		DRAGON)   color=4  ;;
		DARK)     color=3  ;;
		STEEL)    color=8  ;;
		FAIRY)    color=13 ;;
	esac

    if [ "${use_ansi}" ]
    then
	  types="${types} [7;38;5;${color}m ${type} [0m "
	else
	  types="${types} ${type} "
	fi
done

height=$(awk "BEGIN{ print $(echo "${data_pokemon}" | jq --raw-output .height) / 10 }")
weight=$(awk "BEGIN{ print $(echo "${data_pokemon}" | jq --raw-output .weight) / 10 }")
if [ "${use_ansi}" ]
then
  status=$(gum style --align center --width 44 "${types}" "Height: [1m${height}m[0m      Weight: [1m${weight}kg[0m")
else
  status=$(gum style --align center --width 44 "${types}" "Height: ${height}m      Weight: ${weight}kg")
fi

info=$(gum join --vertical "${title}" '' "${status}")

entries=$(gum style --border normal --padding '0 1' --width 42 "$(echo "${data_species}" | \
jq --raw-output 'last(.flavor_text_entries | .[] | select(.language.name == "en")).flavor_text' | tr -d "\n")")

pokemon_path=$(echo "${data_pokemon}" | jq --raw-output .name)
[ "${fetchart}" ] && {
  art=$(gum style "$(curl -fsLS \
    "https://gitlab.com/phoneybadger/pokemon-colorscripts/-/raw/main/colorscripts/small/regular/${pokemon_path}" \
    | sed -e 's/$/[0m/g')")
}


# Display ------------------------------------------------------------------------------------------

terminal_size=$(stty size)
terminal_height=${terminal_size% *}
terminal_width=${terminal_size#* }

prompt_height=${PROMPT_HEIGHT:-1}

print_test() {
	no_color=$(printf '%b' "${1}" | sed -e 's/\x1B\[[0-9;]*[JKmsu]//g')

	[ "$(printf '%s' "${no_color}" | wc --lines)" -gt $(( terminal_height - prompt_height )) ] && return 1
	[ "$(printf '%s' "${no_color}" | wc --max-line-length)" -gt "${terminal_width}" ] && return 1

	gum style --align center --width="${terminal_width}" "${1}" ''
    [ "${use_ansi}" ] && printf '%b' "\033[A"

	exit 0
}


# Landscape layout
group_info_entries=$(gum join --vertical "${info}" '' "${entries}")
[ "${fetchart}" ] && {
  print_test "$(gum join --horizontal --align center "${art}" '  ' "${group_info_entries}")"

  # Portrait layout
  print_test "$(gum join --vertical --align center "${info}" '' "${art}" "${entries}")"
}

# Other layout
print_test "${group_info_entries}"

exit 1
```

## CSV_to_Markdown

The `csv2md` command converts a CSV format document to Markdown. For some very large files on which `csv2md` produced inadequate markdown, [Pandoc](https://pandoc.org) was used.

```shell
#!/bin/bash
#
# csv2md - convert CSV format file to Markdown
#
# Adapted by Ronald Record from:
# https://www.log4code.com/converting-a-csv-file-to-markdown-using-sqlite/
#
# Requires pandoc or sqlite3 3.30.0 or later (preferred)
#
# Usage: ./csv2md [-h] [-r] file.csv
# Where:    -r indicates remove input CSV file after conversion
#
# Output: file.md

help() {
    printf "\nUsage: ./csv2md [-h] [-r] file.csv"
    printf "\nWhere:"
    printf "\n\t-h indicates display this usage message and exit"
    printf "\n\t-r indicates remove input CSV file after conversion\n"
    exit 1
}

# Set any options to defaults
remove=

# Process all the command line options
while :; do
    case $1 in
        # Two hyphens ends the options parsing
        --)
            shift
            break
            ;;
        -h|--help)
            help
            exit
            ;;
        -r|--remove)
            remove=1
            ;;
        -?*)
            echo "The command line option is unknown: $1"
            help
            ;;
        # Anything remaining is treated as content not a parseable option
        *)
            break
            ;;
    esac
    shift
done

[ -f "$1" ] || {
    echo "$1 does not exist or is not a regular file. Exiting."
    exit 1
}

have_sqlite=`type -p sqlite3`
have_pandoc=`type -p pandoc`

inputFile="$1"
baseFile=`echo ${inputFile} | sed -e "s/\.csv//"`

outputFile="${baseFile}.md"
rm -f ${outputFile}

if [ "${have_sqlite}" ]
then
  cmd=$({ grep -v '^#' <<EOF
.headers on
.mode csv
drop table if exists temp_csvFile;
.import ${inputFile} temp_csvFile
.mode markdown
.headers on
.output ${outputFile}
select * from temp_csvFile;
EOF
})

  echo -e "${cmd}" | sqlite3 ''
else
  if [ "${have_pandoc}" ]
  then
    pandoc -f csv -t markdown -o ${outputFile} ${inputFile}
  else
    echo "Neither sqlite3 nor pandoc found. No conversion performed."
  fi
fi

[ "${remove}" ] && rm -f "${inputFile}"
```

## Splitting_large_markdown

Many of the Pokédex CSV files produced very large markdown files. These were split into manageable chunks using the `split_markdown` script I wrote:

```shell
#!/bin/bash
#
# split_markdown - split a markdown file into smaller parts
#
# usage: split_markdown filename.md number_of_parts
#
# assumes the first two lines of the input file are table heading and divider

inputFile=$1
numFiles=$2
numLines=`cat ${inputFile} | wc -l`
baseFile=`echo ${inputFile} | sed -e "s/\.md//"`

split -da 2 \
      -l $((numLines / numFiles)) \
      ${inputFile} ${baseFile}_ --additional-suffix=".md"

rm -f ${inputFile}
firstFile=`echo *_00.md`
table_heading=`head -1 ${firstFile}`
table_divider=`head -2 ${firstFile} | tail -1`

for inputFile in *.md
do
  [ "${inputFile}" == "*.md" ] && continue
  [ "${inputFile}" == "${firstFile}" ] && continue
  echo -e "${table_heading}\n${table_divider}\n$(cat ${inputFile})" > ${inputFile}
done
```

## See also

- [README](../README.md)
