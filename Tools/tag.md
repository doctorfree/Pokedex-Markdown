# tag

```shell
#!/bin/bash
#
# Lines look like:
# |![](Pokemon/thumbnails/001.png)|[Bulbasaur](Pokemon/Bulbasaur.md)|Grass, Poison|Overgrow, (Hidden) Chlorophyll|45|49|49|65|65|45|318|Monster, Grass |

POKEDIR="${HOME}/src/Pokemon/Pokedex-Markdown"
POKEMON="${POKEDIR}/Pokemon"
POKEDEX="${POKEDIR}/pokedex.md"

[ -d "${POKEMON}" ] || {
  echo "${POKEMON} does not exist or is not a directory. Exiting."
  exit 1
}
[ -f "${POKEDEX}" ] || {
  echo "${POKEDEX} does not exist or is not a regular file. Exiting."
  exit 1
}

cd "${POKEMON}"

for inputFile in *.md
do
  [ "${inputFile}" == "*.md" ] && continue
  # Retrieve type and abilities from pokedex.md
  while read line
  do
    echo "${line}" | grep '^|' > /dev/null || continue
    file=`echo ${line} | awk -F '|' ' { print $3 } '`
    file=`echo ${file} | awk -F '(' ' { print $2 } ' | sed -e "s/)//"`
    [ "${POKEDIR}/${file}" == "${POKEMON}/${inputFile}" ] || continue
    pname=`echo ${line} | awk -F '|' ' { print $3 } '`
    pname=`echo ${pname} | awk -F ']' ' { print $1 } ' | sed -e "s/\[//"`
    ptype=`echo ${line} | awk -F '|' ' { print $4 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    pabil=`echo ${line} | awk -F '|' ' { print $5 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    echo "${inputFile}: name=${pname}, type=${ptype}, abilities=${pabil}"
    cat /tmp/t | sed \
      -e "s/__NAME__/${pname}/" \
      -e "s/__TYPE__/${ptype}/" \
      -e "s/__ABIL__/${pabil}/" > /tmp/t$$
    cat /tmp/t$$ ${inputFile} > /tmp/poke$$
    cp /tmp/poke$$ ${inputFile}
    rm -f /tmp/t$$ /tmp/poke$$
  done <<< $(cat ${POKEDEX})
done
```
