# Performance

Performance MUST NOT be sacrificed for premature micro‑optimizations, but obvious inefficiencies are disallowed.

## Collection Handling

- MUST NOT use `+=` on large arrays in loops.
- Use pipelines or strongly‑typed lists instead.

```powershell
$result = foreach ($item in $items) {
    $item * 2
}
```

## Filtering

- Filter as early as possible in the pipeline.
- Avoid unnecessary data retrieval when you only need a subset.
