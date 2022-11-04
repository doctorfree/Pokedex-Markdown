# awk-fixup-img-links

```shell
#!/bin/bash

# Lines look like:
#    | 802 ||| ![](Pokemon/images/XXX.png) |
# awk '{sub("XXX",$2,$4)}1' OFS= /tmp/p > /tmp/q
#
# Lines look like:
# ||Ivysaur|Grass, Poison|Overgrow, null, Chlorophyll|60|62|63|80|80|60|405|Monster, Grass |
# Insert in first field:
# ![](Pokemon/thumbnails/001.png)

num=790
pre=

rm -f /tmp/q
touch /tmp/q
while read line
do
    echo "${line}" | grep XXX.png > /dev/null || {
      echo "${line}" >> /tmp/q
      continue
    }
    name=`echo ${line} | awk -F '|' ' { print $3 } ' | sed -e 's/^ *//' -e 's/ *$//'`
    name=`echo ${name} | awk ' { print $1 } '`
    [ "${name}" == "${pre}" ] || num=$((num + 1))
    pre="${name}"
    if [ ${num} -lt 100 ]
    then
        numstr="0${num}"
    else
        numstr="${num}"
    fi
    echo "${line}" | sed -e "s/XXX/${numstr}/" >> /tmp/q
done <<< $(cat /tmp/p)
```
