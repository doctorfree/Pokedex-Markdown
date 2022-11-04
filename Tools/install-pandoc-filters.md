# install-pandoc-filters

```shell
#!/bin/bash

RELEASE_URL=https://github.com/pandoc/lua-filters/releases/latest
curl -LSs $RELEASE_URL/download/lua-filters.tar.gz | \
    tar --strip-components=1 --one-top-level=/home/ronnie/.local/share/pandoc -zvxf -
```
