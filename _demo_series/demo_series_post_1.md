---
---

Series are enssentially Jekyll Collections. So to add new series is like adding new collections. Say we want to add series `tutorial`

1. config your series in `_config.yml` like

```yaml
collections:
  tutorial:
    output: true
```

2. make corresponding series folder under project root: `_tutorial/`
3. post under series by adding new markdown file into `_tutorial/`

See [Jekyll official doc on `Collections`](https://jekyllrb.com/docs/collections/)
