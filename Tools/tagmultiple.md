# tagmultiple

```shell
#!/bin/bash
#
# Lines look like:
# |![](Pokemon/thumbnails/001.png)|[Bulbasaur](Pokemon/Bulbasaur.md)|Grass, Poison|Overgrow, (Hidden) Chlorophyll|45|49|49|65|65|45|318|Monster, Grass |

POKEDIR="${HOME}/src/Pokemon/Pokedex-Markdown"
POKEMON="${POKEDIR}/Pokemon"
POKEDEX="${POKEDIR}/pokedex.md"
NEEDTAG="Aegislash.md Basculin.md Darmanitan.md Deoxys.md Giratina.md \
         Gourgeist.md Hoopa.md Keldeo.md Landorus.md Lycanroc.md \
         Meloetta.md Meowstic.md Minior.md MissingNo.md Oricorio.md \
         Pumpkaboo.md Shaymin.md Thundurus.md Tornadus.md Wishiwashi.md \
         Wormadam.md Zygarde.md"

[ -d "${POKEMON}" ] || {
  echo "${POKEMON} does not exist or is not a directory. Exiting."
  exit 1
}
[ -f "${POKEDEX}" ] || {
  echo "${POKEDEX} does not exist or is not a regular file. Exiting."
  exit 1
}

cd "${POKEMON}"

for inputFile in ${NEEDTAG}
do
  # Retrieve attributes from pokedex.md
  line=`grep ${inputFile} ${POKEDEX} | tail -1`
  [ "${line}" ] || {
    echo "No entry for ${inputFile}. Skipping."
    continue
  }
  echo "${line}" | grep '^|' > /dev/null || {
    echo "No table entry found in ${line} of ${inputFile}. Skipping."
    continue
  }
  file=`echo ${line} | awk -F '|' ' { print $3 } '`
  file=`echo ${file} | awk -F '(' ' { print $2 } ' | sed -e "s/)//"`
# [ "${POKEDIR}/${file}" == "${POKEMON}/${inputFile}" ] || {
#   echo "Filename mismatch for ${file} and ${inputFile}. Skipping."
#   continue
# }
  pname=`echo ${line} | awk -F '|' ' { print $3 } '`
  pname=`echo ${pname} | awk -F ']' ' { print $1 } ' | sed -e "s/\[//"`
  ptype=`echo ${line} | awk -F '|' ' { print $4 } ' | sed -e 's/^ *//' -e 's/ *$//'`
  pabil=`echo ${line} | awk -F '|' ' { print $5 } ' | sed -e 's/^ *//' -e 's/ *$//'`
  hitp=`echo ${line} | awk -F '|' ' { print $6 } ' | sed -e 's/^ *//' -e 's/ *$//'`
  patk=`echo ${line} | awk -F '|' ' { print $7 } ' | sed -e 's/^ *//' -e 's/ *$//'`
  pdef=`echo ${line} | awk -F '|' ' { print $8 } ' | sed -e 's/^ *//' -e 's/ *$//'`
  pspa=`echo ${line} | awk -F '|' ' { print $9 } ' | sed -e 's/^ *//' -e 's/ *$//'`
  pspd=`echo ${line} | awk -F '|' ' { print $10 } ' | sed -e 's/^ *//' -e 's/ *$//'`
  pspe=`echo ${line} | awk -F '|' ' { print $11 } ' | sed -e 's/^ *//' -e 's/ *$//'`
  ptot=`echo ${line} | awk -F '|' ' { print $12 } ' | sed -e 's/^ *//' -e 's/ *$//'`
  echo "${inputFile}: name=${pname}, type=${ptype}, abilities=${pabil}"
  cat /tmp/b | sed \
      -e "s/__NAME__/${pname}/" \
      -e "s/__TYPE__/${ptype}/" \
      -e "s/__ABIL__/${pabil}/" \
      -e "s/__HITP__/${hitp}/" \
      -e "s/__PATK__/${patk}/" \
      -e "s/__PDEF__/${pdef}/" \
      -e "s/__PSPA__/${pspa}/" \
      -e "s/__PSPD__/${pspd}/" \
      -e "s/__PSPE__/${pspe}/" \
      -e "s/__PTOT__/${ptot}/" > /tmp/b$$
  cat /tmp/b$$ ${inputFile} > /tmp/poke$$
  cp /tmp/poke$$ ${inputFile}
  rm -f /tmp/b$$ /tmp/poke$$
done
```
