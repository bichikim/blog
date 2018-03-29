---
title: Babel env options
date: 2018-03-29 12:11:22
tags: babel babel-env babel-modules
---
## Options
```json
{
  "presets": [[
    "env", {
      "targets": {
        "browser": [
          "> 1%",
          "last 2 versions",
          "not ie <= 8"
        ],
        "modules": "amd"
      }
    }
  ], "stage-1", "stage-2", "stage-3"]
}
```
### targets

### targets.node

### targets.modules