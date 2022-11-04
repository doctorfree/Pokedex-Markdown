# head

```shell
#!/bin/bash

for inputFile in *.md */*.md
do
  [ "${inputFile}" == "*.md" ] && continue
  [ "${inputFile}" == "*/*.md" ] && continue
  grep '^# ' ${inputFile} > /dev/null && continue
  baseFile=`basename ${inputFile}`
  if [ "${inputFile}" == "${baseFile}" ]
  then
    baseFile=`echo ${inputFile} | sed -e "s/\.md//"`
  else
    baseFile=`dirname ${inputFile}`
  fi
  baseFile=`echo ${baseFile} | sed -e "s/_/ /g"`
  heading="# ${baseFile^}"
  cat ${inputFile} | sed -e "/^$/a ${heading}\n" > /tmp/t$$
  cp /tmp/t$$ ${inputFile}
  rm -f /tmp/t$$
done
```
