---
api_name: rollup()
excerpt: Combine multiple MaxN aggregates
topics: [hyperfunctions]
tags: [hyperfunctions, toolkit, maximum]
api:
  license: community
  type: function
  experimental: true
  toolkit: true
  version:
    experimental: 1.12.0
hyperfunction:
  family: minimum and maximum
  type: rollup
  aggregates:
    - max_n()
api_details:
  summary: |
    This aggregate combines the aggregates generated by other `max_n`
    aggregates. Combined with an accessor, it returns the maximum values found 
    across all the aggregated data.
  signatures:
    - language: sql
      code: |
        rollup(
            agg MaxN
        ) MaxN
  parameters:
    required:
      - name: agg
        type: MaxN
        description: The aggregates being combined
    returns:
      - column: rollup
        type: MaxN
        description: >
          An aggregate over all of the contributing values.
---
