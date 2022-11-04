# insert-template2

```shell
#!/bin/bash

# PDIR="${HOME}/src/Pokemon/Pokedex-Markdown/Pokemon"
PDIR="${HOME}/src/Pokemon/Pokedex-Markdown/Test"

FILES="Aegislash.md \
Basculin.md \
Bulbasaur.md \
Darmanitan.md \
Deoxys.md \
Farfetchd.md \
Flabébé.md \
Giratina.md \
Gourgeist.md \
Ho-Oh.md \
Keldeo.md \
Landorus.md \
Lycanroc.md \
Meloetta.md \
Meowstic.md \
Mimikyu.md \
Minior.md \
MissingNo.md \
Nidoran♀.md \
Nidoran♂.md \
Oricorio.md \
Porygon-Z.md \
Pumpkaboo.md \
Shaymin.md \
Thundurus.md \
Tornadus.md \
Wishiwashi.md \
Wormadam.md \
Zygarde.md"

[ -d "${PDIR}" ] || {
    echo "$PDIR does not exist or is not a directory. Exiting."
    exit 1
}
cd "${PDIR}"

for pokemon in ${FILES}
do
    grep "Base Experience" ${pokemon} > /dev/null && continue
    loname=`echo ${pokemon} | sed -e "s/\.md//" | tr '[:upper:]' '[:lower:]'`
    poke=`grep ${loname} /tmp/list | tail -1`
    species=`echo ${poke} | awk -F '|' ' { print $4 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    height=`echo ${poke} | awk -F '|' ' { print $5 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    weight=`echo ${poke} | awk -F '|' ' { print $6 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    bexp=`echo ${poke} | awk -F '|' ' { print $7 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    cat /tmp/template.md | sed \
        -e "s/__SPECIES__/${species}/g" \
        -e "s/__HEIGHT__/${height}/g" \
        -e "s/__WEIGHT__/${weight}/g" \
        -e "s/__BEXP__/${bexp}/g" > /tmp/__insert__
    sed '/## See also/e cat /tmp/__insert__' ${pokemon} > /tmp/foo$$
    cp /tmp/foo$$ ${pokemon}
    rm -f /tmp/foo$$ /tmp/__insert__
done
```
