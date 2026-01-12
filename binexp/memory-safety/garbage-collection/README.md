# Garbage Collection

Garbage Collection is a form of **automatic memory management**, used by (often) higher-level programming languages to remove the need for complicated manual memory management. As a result, manual memory management is **not implemented**, and the developer relies on the garbage collection to handle memory, preventing bugs such as [UAFs](../../../types/heap/use-after-free.md) or [Double-Frees](../../../types/heap/double-free/).

The two main types are [Tracing Garbage Collection](tracing-garbage-collection.md) (which is often just referred to as "Garbage Collection") and [Automatic Reference Counting](automatic-reference-counting.md).
