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
        //Console.WriteLine(n);

        return n;
    }
```