---
category: demo
tags: [tutorial]
usemath: [latex, ascii]
---

## Blogging

You can begin writting your posts under `/_posts` folder. See [Jekyll doc on posts](https://jekyllrb.com/docs/posts/).

Posts are automatically grouped under site's post/category and post/tag menu. You can define post's category and tags in the post's [front matter](https://jekyllrb.com/docs/front-matter/), using `category` and `tags` front matter block.

## Math Notation

Textalic support math notation via `mathjax`.

There is two flavor to write math notation:

### Latex Math

Enable this by add an `usemath: latex` front matter in your post. Then you can wrap math notation inside `$$`. For example:

`$$E=mc^2$$` will be rendered as

$$E=mc^2$$

### Asciimath

Enable this by add an `usemath: ascii` front matter in your post. Then you can wrap math notation inside \\`. For example:

```
\`sum_(i=1)^n i^3=((n(n+1))/2)^2\`
```

will be rendered as

\`sum_(i=1)^n i^3=((n(n+1))/2)^2\`

Checkout ascii math notation at <http://asciimath.org/>

---

You can use both by adding front matter `usemath: [latex, ascii]`

### Series

Sereis are implemented using [Jekyll's collections](https://jekyllrb.com/docs/collections/). So to add new series, following these steps:

1. Define a new collection in `_config.yml`, under `collections` configuration block

    ```yaml
    collections:
      demo_series:
        output: true
    ```

    Note that to make the change to `_config.yml` take effects, you **need to restart jekyll**.

2. Create the series (aka collection) folder `/_demo_series`. Note that the **folder name begin with `_`**

3. By adding new post under series folder, you add post under the corresponding series

## Deployment

See [Jekyll doc on deployment](https://jekyllrb.com/docs/deployment/)

## That's it

Happy blogging!