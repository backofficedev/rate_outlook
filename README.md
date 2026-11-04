# Rate Outlook

A pipeline that builds on two others. It never reads their folders: it pins their snapshots in
`chartbook.lock`, and reads their dataframes by name,

```python
curve = data.load("treasury_curve:zero_coupon_yields")
macro = data.load("macro_basics:macro_series")
```

so its numbers do not change until `chartbook update` moves a pin. It derives the term spread,
charts it beside the unemployment rate, and fits a small model to it, whose page names the exact
snapshot of the yield curve it was trained on.

## Build it

```console
pip install chartbook doit
chartbook update          # move the pins to the snapshots in your store: the committed chartbook.lock names the author's
chartbook run --snapshot
```

`chartbook.lock` is committed, and names the exact snapshots of `treasury_curve` and
`macro_basics` this pipeline was last built on. Those are the author's; yours are the ones you
built, so `chartbook update` moves the pins to them, and `git diff chartbook.lock` shows every
hash that moved. This pipeline is part of the ChartBook tutorial,
[Explore a Catalog](https://backofficedev.github.io/chartbook/tutorials/explore-a-catalog.html).
