Ruby example for benchmarking with [benchmark-ips][tool]
========================================================

- [Workflow for this example](../../.github/workflows/ruby.yml)
- [Action log of this example](https://github.com/benchmark-action/github-action-benchmark/actions?query=workflow%3A%22Ruby+Example+with+benchmark-ips%22)
- [Benchmark results on GitHub pages](https://benchmark-action.github.io/github-action-benchmark/dev/bench/)

This directory shows how to use [`github-action-benchmark`](https://github.com/benchmark-action/github-action-benchmark)
with [benchmark-ips][tool].

## Run benchmarks

Official documentation for usage of benchmark-ips:

https://github.com/evanphx/benchmark-ips

Add `benchmark-ips` to your `Gemfile`:

```ruby
gem 'benchmark-ips'
```

Prepare script `bench.rb` as follows:

e.g.

```ruby
require 'benchmark/ips'

Benchmark.ips do |x|
  x.report('some test case') do
    # ...
  end
end
```

benchmark-ips prints its results to stdout. This action extracts the iterations per second
from the "Calculating" section of that output. `x.compare!` and `:raw` format are also supported.
Since the measured value is iterations per second, a bigger value is better.

Run the script in workflow:

e.g.

```yaml
- name: Run benchmark
  run: bundle exec ruby bench.rb | tee output.txt
```

## Process benchmark results

Store the benchmark results with step using the action. Please set `benchmarkips` to `tool` input.

```yaml
- name: Store benchmark result
  uses: benchmark-action/github-action-benchmark@v1
  with:
    tool: 'benchmarkips'
    output-file-path: output.txt
```

Please read ['How to use' section](https://github.com/benchmark-action/github-action-benchmark#how-to-use) for common usage.

[tool]: https://github.com/evanphx/benchmark-ips
