# fixmeta

```shell
#!/bin/bash

# PDIR="${HOME}/src/Pokemon/Pokedex-Markdown/Pokemon"
PDIR="${HOME}/src/Pokemon/Pokedex-Markdown/Test"

[ -d "${PDIR}" ] || {
    echo "$PDIR does not exist or is not a directory. Exiting."
    exit 1
}
cd "${PDIR}"

for poke in *.md
do
    grep "special-" ${poke} > /dev/null || continue
    cat ${poke} | sed -e "s/special-/special/g" > /tmp/foo$$
    cp /tmp/foo$$ ${poke}
    rm -f /tmp/foo$$
done
```
