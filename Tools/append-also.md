# append-also

```shell
#!/bin/bash

POKEMON="${HOME}/src/Pokemon/Pokedex-Markdown/Pokemon"

[ -d ${POKEMON} ] || {
    echo "${POKEMON} does not exist or is not a directory. Exiting."
    exit 1
}

cd ${POKEMON}

for poke in *.md
do
    grep "See also" ${poke} > /dev/null || continue
    cat ${poke} | sed -e "s/\[List of Pok/- \[List of Pok/" > /tmp/p$$
    cat /tmp/p$$ /tmp/m > ${poke}
    rm -f /tmp/p$$
done
```
