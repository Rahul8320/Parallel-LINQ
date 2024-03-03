# Parallel-LINQ

- AsParallel
```c#
using System.Diagnostics;

var stopwatch = Stopwatch.StartNew();

var collection = Enumerable.Range(0, 10)
    .AsParallel()
    .Select(HeavyComputation);

foreach (var _ in collection);
stopwatch.Stop();
Console.WriteLine($"Execution Time: {stopwatch.ElapsedMilliseconds}");

int HeavyComputation(int n)
{
    for(int j = 0;j < 100000000; j++)
    {
        n += j;
    }
    return n;
}
```

- WithDegreeOfParallelism
```c#
using System.Collections.Concurrent;
using System.Diagnostics;

var stopwatch = Stopwatch.StartNew();

ConcurrentDictionary<int, List<int>> threadsMap = [];

var collection = Enumerable.Range(0, 10)
    .AsParallel()
    .WithDegreeOfParallelism(2)
    .Select(HeavyComputation);

foreach (var _ in collection);
stopwatch.Stop();
Console.WriteLine($"Execution Time: {stopwatch.ElapsedMilliseconds}");

foreach (var item in threadsMap)
{
    Console.Write(item.Key + " -> ");
    foreach (var thread in item.Value)
    {
        Console.Write(thread + " ");
    }
    Console.WriteLine();
}

int HeavyComputation(int n)
{
    threadsMap.AddOrUpdate(
        key: Environment.CurrentManagedThreadId,
        addValue: [n],
        updateValueFactory: (key, values) => [.. values, n]);

    for(int j = 0;j < 100000000; j++)
    {
        n += j;
    }
    return n;
}
```