# pre

```shell
#!/bin/bash

firstFile=`echo *000.md`
table_heading=`head -1 ${firstFile}`
echo "${table_heading}"

for inputFile in *.md
do
  [ "${inputFile}" == "*.md" ] && continue
  [ "${inputFile}" == "${firstFile}" ] && continue
  echo -e "${table_heading}\n$(cat ${inputFile})" > ${inputFile}
done
```
