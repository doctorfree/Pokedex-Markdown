# prepend-name

```shell
#!/bin/bash

# PDIR="${HOME}/src/Pokemon/Pokedex-Markdown"
PDIR="${HOME}/src/Pokemon/Pokedex-Markdown/Test"
pokemon="${PDIR}/pokemon.md"

[ -d "${PDIR}" ] || {
    echo "$PDIR does not exist or is not a directory. Exiting."
    exit 1
}
cd "${PDIR}"

grep '|||' ${pokemon} > /tmp/pre$$
cat /tmp/pre$$ | while read line
do
    num=`echo "${line}" | awk -F '|' ' { print $2 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    [ -f /tmp/${num} ] || {
        echo "/tmp/$num does not exist. Skipping."
        continue
    }
    sed "/^|${num}|/e cat /tmp/${num}" ${pokemon} > /tmp/foo$$
    cp /tmp/foo$$ ${pokemon}
    rm -f /tmp/foo$$
done
```
