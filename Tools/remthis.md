# remthis

```shell
#!/bin/bash
#

POKEDIR="${HOME}/src/Pokemon/Pokedex-Markdown"
POKEMON="${POKEDIR}/Pokemon"

[ -d "${POKEMON}" ] || {
  echo "${POKEMON} does not exist or is not a directory. Exiting."
  exit 1
}

cd "${POKEMON}"

for inputFile in *.md
do
  [ "${inputFile}" == "*.md" ] && continue
    cat ${inputFile} | grep -v "Height is measured" > /tmp/poke$$
    cat /tmp/poke$$ | grep -v "Weight is measured" > /tmp/w$$
    cat /tmp/w$$ | sed -e "s/\*\*Height\*\*/\*\*Height dm\*\*/" > /tmp/poke$$
    cat /tmp/poke$$ | sed -e "s/\*\*Weight\*\*/\*\*Weight hg\*\*/" > /tmp/w$$
    cp /tmp/w$$ ${inputFile}
    rm -f /tmp/w$$ /tmp/poke$$
done
```
