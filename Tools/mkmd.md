# mkmd

```shell
#!/bin/bash

rm -f /tmp/pm.md
touch /tmp/pm.md

cat /tmp/pm | while read i
do
  name=`basename $i`
  name=`echo ${name} | sed -e "s/\.md//" -e "s/_/ /g"`
  name="${name^}"
  echo "- [${name}]($i)" >> /tmp/pm.md
done
```
