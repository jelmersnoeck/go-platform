# Metrics - Instrumentation interface
 
Provides a high level abstraction to instrument metrics.

By default we support the telegraf/statsd interface which supports influxdb labels.

## Interface

```
type Fields map[string]string

// Metrics provides a way to instrument application data
type Metrics interface {
        Init(...Option) error
        Counter(id string) Counter
        Gauge(id string) Gauge
        Histogram(id string) Histogram
        // Start/Stop a batched collector
        Start() error
        Stop() error
        // Name
        String() string
}

type Counter interface {
        // Increment by the given value
        Incr(d uint64)
        // Decrement by the given value
        Decr(d uint64)
        // Reset the counter
        Reset()
        // Label the counter
        WithFields(f Fields) Counter
}

type Gauge interface {
        // Set the gauge value
        Set(d int64)
        // Reset the gauge
        Reset()
        // Label the gauge
        WithFields(f Fields) Gauge
}

type Histogram interface {
        // Record a timing
        Record(d int64)
        // Reset the histogram
        Reset()
        // Label the histogram
        WithFields(f Fields) Histogram
}
```

## Supported Metric stores

- Telegraf/StatsD
