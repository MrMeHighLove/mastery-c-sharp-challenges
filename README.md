# 50 Mastery Challenges for High-Performance C#

These are categorized by the specific "low-level" skill they force you to learn. Solving these will put you in the top 1% of .NET engineers.

## I. Memory Slicing & Span (1-10)

1. **Zero-Copy Parser**: Parse a CSV line using `ReadOnlySpan<char>` without calling `string.Split`.
2. **Protocol Header Extractor**: Read a TCP byte stream and extract a header using `Span<byte>` without creating a new array.
3. **Hex to Byte Converter**: Write a method that converts a hex string to bytes using `Span`.
4. **In-Place String Reversal**: Reverse a `Span<char>` without allocating a second string.
5. **Sub-string Search**: Implement a "Contains" method that works on `ReadOnlySpan` and is faster than the built-in one.
6. **StackAlloc Buffer**: Use `stackalloc` to process small arrays (<1KB) to avoid the Heap entirely.
7. **Custom Guid Parser**: Parse a Guid from a `ReadOnlySpan<char>` manually.
8. **Base64 Encoder**: Encode a byte span to a char span without allocating a string.
9. **Span-based String Builder**: Create a `ValueStringBuilder` (using `Span`) for local string concatenation.
10. **Memory-Mapped Files**: Read a 10GB file by "mapping" it to memory and using `Span` to access chunks.

## II. Array Pooling & Object Reuse (11-20)

11. **The ArrayPool Buffer**: Use `ArrayPool<byte>.Shared` to rent buffers for file I/O and return them.
12. **Custom Object Pool**: Build a generic `ObjectPool<T>` for high-frequency objects (like Game Entities or DB Connections).
13. **ArrayPool with Memory**: Create a wrapper that rents an array but exposes it as `Memory<T>`.
14. **JSON Document Scraper**: Use `JsonDocument` (which uses pooled memory internally) to extract one field from a 50MB JSON.
15. **Reusable TaskCompletionSource**: Use the `ManualResetValueTaskSourceCore` to reuse "task" objects.
16. **Avoiding Closure Allocations**: Refactor a LINQ query to use a `foreach` loop to avoid hidden delegate allocations.
17. **The "params" Trap**: Rewrite a method taking `params int[]` to take `ReadOnlySpan<int>` to avoid the array allocation at the call site.
18. **Pooled StringBuilder**: Implement a pool for `StringBuilder` instances.
19. **The Zero-Alloc Event**: Implement a custom event system that doesn't allocate `EventArgs`.
20. **Struct vs Class**: Refactor a high-frequency "Point" or "Vector" class into a `readonly struct` and measure the GC impact.

## III. System.IO.Pipelines & High-Level I/O (21-30)

21. **The Pipe Reader**: Read a line-by-line stream using `PipeReader` to handle "partial reads" automatically.
22. **Delimiter Finder**: Efficiently find `\n` in a `ReadOnlySequence<byte>` (multi-segment buffer).
23. **High-Throughput Socket**: Build a TCP Echo server using `System.IO.Pipelines`.
24. **Streaming Multi-part Upload**: Process a file upload in ASP.NET Core without saving the whole file to disk or memory.
25. **Custom Stream Wrapper**: Write a `Stream` decorator that counts bytes without extra buffering.
26. **SequenceReader Mastery**: Use `SequenceReader<byte>` to parse a binary protocol (like Redis or MQTT).
27. **Async Enumerable Streaming**: Stream database rows to a JSON response using `IAsyncEnumerable`.
28. **Direct File Copy**: Use `File.OpenHandle` and `RandomAccess` for lowest-level OS file access.
29. **Non-Blocking Logs**: Build a background log worker using `Channel<T>` for high-frequency writes.
30. **Stream.ReadAsync vs Read**: Implement a buffer loop that uses `ValueTask`-based `ReadAsync` to minimize state machine allocations.

## IV. Structs, Layouts & SIMD (31-40)

31. **Explicit Layout**: Use `[StructLayout(LayoutKind.Explicit)]` to create a "Union" type (C-style).
32. **The Padding Problem**: Optimize a struct's field order to reduce its size in memory.
33. **SIMD Vectorization**: Use `Vector<T>` to sum an array of 1 million integers 4x faster.
34. **Bulk Comparison**: Use `Vector128` to check if two byte arrays are equal.
35. **Bit Manipulation**: Use `BitOperations.PopCount` or `LeadingZeroCount` for high-speed bit math.
36. **Unsafe Pointer Math**: Use `fixed` pointers to iterate through an array (C-style) for maximum speed.
37. **Ref Structs**: Create a `ref struct` that can only exist on the stack and never be boxed.
38. **Unsafe.As**: Use `Unsafe.As<T, U>` to "cast" a byte array to a struct without copying.
39. **Custom Allocator**: Use `NativeMemory.Alloc` to allocate memory outside the .NET Garbage Collector.
40. **Function Pointers**: Use `delegate* unmanaged<...>` to call a function with zero delegate overhead.

## V. Advanced .NET Internals (41-50)

41. **Interlocked Counter**: Implement a thread-safe counter without using `lock`.
42. **SpinLock vs Lock**: Implement a low-latency cache using `SpinLock`.
43. **ValueTask Optimization**: Convert an async `Task` method to `ValueTask` and ensure it only allocates when it actually goes async.
44. **The "Wait" Strategy**: Implement a No-Op loop vs `Thread.Yield` vs `Thread.Sleep` for high-frequency polling.
45. **String.Create**: Use `string.Create` with a state object to build a string without an intermediate `StringBuilder`.
46. **SearchValues<T>**: Use the new `SearchValues` class (C# 12+) for high-speed character searching.
47. **FrozenDictionary**: Convert a static dictionary to a `FrozenDictionary` for O(1) lookups that are 2x faster.
48. **Method Inlining**: Use `[MethodImpl(MethodImplOptions.AggressiveInlining)]` on a hot-path method and check the assembly.
49. **Low-Level Formatting**: Use `ISpanFormattable` to format data directly into a buffer.
50. **The GC Audit**: Use `GC.GetAllocatedBytesForCurrentThread()` to prove a specific method call allocates exactly zero bytes.
