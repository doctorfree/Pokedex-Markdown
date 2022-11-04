# addtags

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
  # Retrieve stats from pokedex.md
  while read line
  do
    echo "${line}" | grep '^|' > /dev/null || continue
    file=`echo ${line} | awk -F '|' ' { print $3 } '`
    file=`echo ${file} | awk -F '(' ' { print $2 } ' | sed -e "s/)//"`
    [ "${POKEDIR}/${file}" == "${POKEMON}/${inputFile}" ] || continue
    hitp=`echo ${line} | awk -F '|' ' { print $6 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    patk=`echo ${line} | awk -F '|' ' { print $7 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    pdef=`echo ${line} | awk -F '|' ' { print $8 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    pspa=`echo ${line} | awk -F '|' ' { print $9 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    pspd=`echo ${line} | awk -F '|' ' { print $10 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    pspe=`echo ${line} | awk -F '|' ' { print $11 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    ptot=`echo ${line} | awk -F '|' ' { print $12 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    echo "${inputFile}: hitp=${hitp}, patk=${patk}, pdef=${pdef}"
    cat /tmp/a | sed \
      -e "s/__HITP__/${hitp}/" \
      -e "s/__PATK__/${patk}/" \
      -e "s/__PDEF__/${pdef}/" \
      -e "s/__PSPA__/${pspa}/" \
      -e "s/__PSPD__/${pspd}/" \
      -e "s/__PSPE__/${pspe}/" \
      -e "s/__PTOT__/${ptot}/" > /tmp/a$$
    cat ${inputFile} | sed -e "/^abilities:/ r /tmp/a$$" > /tmp/poke$$
    cp /tmp/poke$$ ${inputFile}
    rm -f /tmp/a$$ /tmp/poke$$
  done <<< $(cat ${POKEDEX})
done
```
