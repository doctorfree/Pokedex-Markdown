# append-info

```shell
#!/bin/bash

# Lines look like:
#    |001|001|Bulbasaur|Grass-Poison |
#
# | **Id** | **Name** | **Species Id** | **Height** | **Weight** | **Base Experience** | **Order** | **Is Default** |
# | 1   | bulbasaur      | 1    | 7  | 69  | 64    | 1   | 1    |

PDIR="${HOME}/src/Pokemon/Pokedex-Markdown/Pokemon"

[ -d "${PDIR}" ] || {
    echo "$PDIR does not exist or is not a directory. Exiting."
    exit 1
}
cd "${PDIR}"

cat /tmp/list | while read poke
do
    name=`echo ${poke} | awk -F '|' ' { print $3 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    name="${name^}"
    [ "${name}" == "Bulbasaur" ] && continue
    [ -f "${name}.md" ] || continue
    index=`echo ${poke} | awk -F '|' ' { print $2 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    species=`echo ${poke} | awk -F '|' ' { print $4 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    height=`echo ${poke} | awk -F '|' ' { print $5 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    weight=`echo ${poke} | awk -F '|' ' { print $6 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    bexp=`echo ${poke} | awk -F '|' ' { print $7 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    cat /tmp/template.md | sed \
        -e "s/__NAME__/${name}/g" \
        -e "s/__ID__/${index}/g" \
        -e "s/__SPECIES__/${species}/g" \
        -e "s/__HEIGHT__/${height}/g" \
        -e "s/__WEIGHT__/${weight}/g" \
        -e "s/__BEXP__/${bexp}/g" > /tmp/__insert__
    echo "" >> /tmp/__insert__
    sed '/## See also/e cat /tmp/__insert__' ${name}.md > /tmp/foo$$
    cp /tmp/foo$$ ${name}.md
    rm -f /tmp/foo$$ /tmp/__insert__
done
```
