# add-link

```shell
#!/bin/bash

# Lines look like:
# |001|001|Bulbasaur|Grass-Poison |
# Modify name field with link like:
# [Bulbasaur](Pokemon/Bulbasaur.md)

POKEMON="${HOME}/src/Pokemon/Pokedex-Markdown/Pokemon"

rm -f /tmp/q
touch /tmp/q
while read line
do
    echo "${line}" | grep '^|' > /dev/null || {
      echo "${line}" >> /tmp/q
      continue
    }
    name=`echo ${line} | awk -F '|' ' { print $3 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    name=`echo ${name} | awk ' { print $1 } '`
    [ -f ${POKEMON}/${name}.md ] || {
      echo "${line}" >> /tmp/q
      continue
    }
    echo "${line}" | sed -e "s%${name}%[${name}](Pokemon/${name}.md)%" >> /tmp/q
done <<< $(cat /tmp/p)
```
