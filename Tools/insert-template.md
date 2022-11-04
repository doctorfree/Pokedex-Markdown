# insert-template

```shell
#!/bin/bash

PDIR="${HOME}/src/Pokemon/Pokedex-Markdown/Pokemon"

[ -d "${PDIR}" ] || {
    echo "$PDIR does not exist or is not a directory. Exiting."
    exit 1
}
cd "${PDIR}"

for poke in *.md
do
    grep "Special Attack" ${poke} > /dev/null && continue
    sed '/## See also/e cat /tmp/__template__' ${poke} > /tmp/foo$$
    cp /tmp/foo$$ ${poke}
    rm -f /tmp/foo$$
done
```
