[![JSON Fliox](https://raw.githubusercontent.com/friflo/Friflo.Json.Fliox/main/docs/images/Json-Fliox.svg)](https://github.com/friflo/Friflo.Json.Fliox/blob/main/Engine/README.md)   ![splash](https://raw.githubusercontent.com/friflo/Friflo.Json.Fliox/main/docs/images/paint-splatter.svg)

[![nuget](https://img.shields.io/nuget/v/Friflo.Engine.ECS?logo=nuget&logoColor=white)](https://www.nuget.org/packages/Friflo.Engine.ECS)
[![published](https://img.shields.io/badge/published-2024-blue.svg)](https://www.nuget.org/packages/Friflo.Engine.ECS/1.0.0)
[![license](https://img.shields.io/badge/license-LGPL-blue.svg)](https://github.com/friflo/Friflo.Json.Fliox/blob/main/LICENSE)
[![codecov](https://img.shields.io/codecov/c/gh/friflo/Friflo.Json.Fliox?logo=codecov&logoColor=white&label=codecov)](https://app.codecov.io/gh/friflo/Friflo.Json.Fliox/tree/main/Engine%2Fsrc%2FECS)
[![CI-Engine](https://img.shields.io/github/actions/workflow/status/friflo/Friflo.Json.Fliox/.github%2Fworkflows%2Fengine.yml?logo=github&logoColor=white&label=CI)](https://github.com/friflo/Friflo.Json.Fliox/actions/workflows/engine.yml)
[![docs](https://img.shields.io/badge/docs-C%23%20API-blue.svg)](https://github.com/friflo/Friflo.Engine-docs/blob/main/README.md)
[![stars](https://img.shields.io/github/stars/friflo/Friflo.Json.Fliox.svg)](https://github.com/friflo/Friflo.Json.Fliox)


# Friflo.Engine.ECS

Currently fastest 🔥 ECS implementation in C# / .NET - using **Ecs.CSharp.Benchmark** as reference.  
See benchmark results - Mac Mini M2 - at the bottom of this page.  
This ECS is an Archetype / AoS based Entity Component System. See: [ECS - Wikipedia](https://en.wikipedia.org/wiki/Entity_component_system).   

*Feature highlights*
- Simple API.
- High-performance 🔥 and compact ECS implementation - Friflo.Engine.ECS.dll size 250 KB
- Subscribe events of specific or all entities.
- JSON Serialization.
- SIMD Support - optional. Multi thread capable and remainder loop free.
- Supports .NET Standard 2.1 .NET 5 .NET 6 .NET 7 .NET 8  
  WASM / WebAssembly, Unity (Mono & AOT/IL2CPP), Godot and MonoGame.
- No use of unsafe code. See [Wiki ⋅ Library](https://github.com/friflo/Friflo.Json.Fliox/wiki/Library).

Complete feature list at [Wiki ⋅ Features](https://github.com/friflo/Friflo.Json.Fliox/wiki/Features).

Get package on [nuget](https://www.nuget.org/packages/Friflo.Engine.ECS/) or use the dotnet CLI.
```
dotnet add package Friflo.Engine.ECS
```


# Contents

* [Demos](#demos)
* [Examples](#examples)
  - [Hello World](#hello-world)
  - [Systems](#systems)
* [Wiki](#wiki)
* [Benchmarks](#ecs-benchmarks)

<br/>


# Demos

**Interactive Browser Demo** showing MonoGame WebAssembly integration. [Try online Demo](https://sdl-wasm-sample-web.vercel.app/docs/MonoGame/).  

<span>
  <a href="https://sdl-wasm-sample-web.vercel.app/docs/MonoGame/"               ><img src="https://raw.githubusercontent.com/friflo/Friflo.Engine-docs/main/docs/images/MonoGame-wasm.png"  width="320" height="197"/></a>
  <a href="https://github.com/friflo/Friflo.Engine.ECS-Demos/tree/main/Unity"   ><img src="https://raw.githubusercontent.com/friflo/Friflo.Engine-docs/main/docs/images/Unity.png"          width="320" height="197"/></a>
  <a href="https://github.com/friflo/Friflo.Engine.ECS-Demos/tree/main/Godot"   ><img src="https://raw.githubusercontent.com/friflo/Friflo.Engine-docs/main/docs/images/Godot.png"          width="320" height="197"/></a>
</span>

*Note:* WebGL has currently poor render performance.  
*Desktop performance of Demos:* Godot 202 FPS, Unity 100 FPS at 65536 entities.

Example Demos for **Windows**, **macOS** & **Linux** available as projects for **Godot**, **MonoGame** and **Unity**.  
See [Demos · GitHub](https://github.com/friflo/Friflo.Engine.ECS-Demos)

<br/>

# Examples

This section contains two typical use cases when using an ECS.  
More examples are in the GitHub Wiki.

[**Examples - General**](https://github.com/friflo/Friflo.Json.Fliox/wiki/Examples-~-General)  
Explain fundamental ECS types like *Entity*, *Component*, *Tag*, *Command Buffer*, ... and how to use them.

[**Examples - Optimization**](https://github.com/friflo/Friflo.Json.Fliox/wiki/Examples-~-Optimization)  
Provide techniques how to improve ECS performance.

## Hello World

The hello world examples demonstrates the creation of a world, some entities with components  
and their movement using a simple `ForEachEntity()` call.  

```csharp
public struct Velocity : IComponent { public Vector3 value; }

public static void HelloWorld()
{
    var world = new EntityStore();
    for (int n = 0; n < 10; n++) {
        world.CreateEntity(new Position(n, 0, 0), new Velocity{ value = new Vector3(0, n, 0)});
    }
    var query = world.Query<Position, Velocity>();
    query.ForEachEntity((ref Position position, ref Velocity velocity, Entity entity) => {
        position.value += velocity.value;
    });
}
```
In case of moving (updating) thousands or millions of entities an optimized approach can be used.  
See:
[Enumerate Query Chunks](https://github.com/friflo/Friflo.Json.Fliox/wiki/Examples-~-Optimization#enumerate-query-chunks),
[Parallel Query Job](https://github.com/friflo/Friflo.Json.Fliox/wiki/Examples-~-Optimization#parallel-query-job) and
[Query Vectorization - SIMD](https://github.com/friflo/Friflo.Json.Fliox/wiki/Examples-~-Optimization#query-vectorization---simd).  
All query optimizations are using the same `query` but with different enumeration techniques.



## Systems

Systems are new in **Friflo.Engine.ECS v2**  
[![nuget](https://img.shields.io/nuget/vpre/Friflo.Engine.ECS?logo=nuget&logoColor=white&color=orange)](https://www.nuget.org/packages/Friflo.Engine.ECS/absoluteLatest)

Systems in ECS are typically queries.  
So you can still use the `world.Query<Position, Velocity>()` shown in the "Hello World" example.  

Using Systems is optional but they have some significant advantages:

- It enables chaining multiple decoupled [QuerySystem](https://github.com/friflo/Friflo.Engine-docs/blob/main/api/QuerySystem.md) classes.

- System fields can be used as parameters in `OnUpdate()`.

- Each system is added to a [SystemGroup](https://github.com/friflo/Friflo.Engine-docs/blob/main/api/SystemGroup.md).
  [SystemRoot](https://github.com/friflo/Friflo.Engine-docs/blob/main/api/SystemRoot.md)
  is also a [SystemGroup](https://github.com/friflo/Friflo.Engine-docs/blob/main/api/SystemGroup.md).  
  Each group provide a [CommandBuffer](https://github.com/friflo/Friflo.Json.Fliox/wiki/Examples-~-Optimization#commandbuffer).

- Systems have performance monitoring build-in. If enabled systems detected as bottleneck can be optimized.

- Multiple worlds can be added to a single  [SystemRoot](https://github.com/friflo/Friflo.Engine-docs/blob/main/api/SystemRoot.md) instance.  
  `root.Update()` will execute every system on all worlds.


```csharp
public static void HelloSystem()
{
    var world = new EntityStore();
    for (int n = 0; n < 10; n++) {
        world.CreateEntity(new Position(n, 0, 0), new Velocity(), new Scale3());
    }
    var root = new SystemRoot(world) {
        new MoveSystem(),
    //  new PulseSystem(),
    //  new ... multiple systems can be added. The execution order still remains clear.
    };
    root.Update(default);
}
        
class MoveSystem : QuerySystem<Position, Velocity>
{
    protected override void OnUpdate() {
        Query.ForEachEntity((ref Position position, ref Velocity velocity, Entity entity) => {
            position.value += velocity.value;
        });
    }
}
```

A valuable strength of an ECS is establishing a clear and decoupled code structure.  
Adding the `PulseSystem` below to the `SystemRoot` above is trivial.

```csharp
class PulseSystem : QuerySystem<Scale3>
{
    float frequency = 4f;
    
    protected override void OnUpdate() {
        Query.ForEachEntity((ref Scale3 scale, Entity entity) => {
            scale.value = Vector3.One * (1 + 0.2f * MathF.Sin(frequency * Tick.time));
        });
    }
}
```

<br/>


# Wiki

The **GitHub Wiki** provide you detailed information about the ECS and illustrate them by examples.

- [**Examples - General**](https://github.com/friflo/Friflo.Json.Fliox/wiki/Examples-~-General)  
  Explain fundamental ECS types like *Entity*, *Component*, *Tag*, *Command Buffer*, ... and show you how to use them.

- [**Examples - Optimization**](https://github.com/friflo/Friflo.Json.Fliox/wiki/Examples-~-Optimization)  
  Provide you techniques how to improve ECS performance.

- [**Extensions**](https://github.com/friflo/Friflo.Json.Fliox/wiki/Extensions)  
  Projects extending Friflo.Engine.ECS with additional features.
  
- [**Features**](https://github.com/friflo/Friflo.Json.Fliox/wiki/Features)  
  Integration possibilities, a complete feature list and performance characteristics 🔥.

- [**Library**](https://github.com/friflo/Friflo.Json.Fliox/wiki/Library)  
  List supported platforms, properties of the assembly dll and build statistics.

- [**Release Notes**](https://github.com/friflo/Friflo.Json.Fliox/wiki/Release-Notes)  
  List of changes of every release available on nuget.

<br/>



# ECS Benchmarks

Two benchmarks - subset of [Ecs.CSharp.Benchmark - 2024-02-16](https://github.com/Doraku/Ecs.CSharp.Benchmark/tree/da28d170988949ee36eab62258c6130d473e70ac)
running on a Mac Mini M2.

Made a subset as the other benchmarks are similar only with different parameters.

1. Create 100.000 entities with three components
2. Update 100.000 entities with two components


## 1. Create 100.000 entities with three components

| Method            |  | Mean      | Error     | StdDev    | Gen0      | Gen1      | Gen2      | Allocated   |
|------------------ |--|----------:|----------:|----------:|----------:|----------:|----------:|------------:|
| Arch              |  |  2.411 ms | 0.0370 ms | 0.0657 ms |         - |         - |         - |  3948.49 KB |
| SveltoECS         |  | 28.246 ms | 0.5175 ms | 0.4840 ms |         - |         - |         - |     4.97 KB |
| DefaultEcs        |  |  5.931 ms | 0.1179 ms | 0.2685 ms | 2000.0000 | 2000.0000 | 2000.0000 | 19526.04 KB |
| FlecsNet          |  | 14.896 ms | 0.1574 ms | 0.1229 ms |         - |         - |         - |     3.81 KB |
| FrifloEngineEcs   |🔥|  1.293 ms | 0.0116 ms | 0.0097 ms | 1000.0000 | 1000.0000 | 1000.0000 |  6758.76 KB |
| HypEcs            |  | 22.243 ms | 0.1328 ms | 0.1178 ms | 8000.0000 | 3000.0000 | 3000.0000 | 68762.52 KB |
| LeopotamEcsLite   |  |  2.646 ms | 0.0520 ms | 0.0884 ms | 2000.0000 | 2000.0000 | 2000.0000 | 11253.58 KB |
| LeopotamEcs       |  |  7.944 ms | 0.1398 ms | 0.1554 ms | 2000.0000 | 2000.0000 | 2000.0000 | 15741.98 KB |
| MonoGameExtended  |  | 25.024 ms | 0.0763 ms | 0.1232 ms | 4000.0000 | 3000.0000 | 3000.0000 | 30162.07 KB |
| Morpeh_Direct     |  | 90.162 ms | 0.2032 ms | 0.1801 ms | 9000.0000 | 5000.0000 | 2000.0000 | 83805.52 KB |
| Morpeh_Stash      |  | 30.655 ms | 0.3532 ms | 0.3131 ms | 4000.0000 | 2000.0000 | 1000.0000 | 44720.38 KB |
| RelEcs            |  | 56.156 ms | 0.4419 ms | 0.4134 ms | 9000.0000 | 4000.0000 | 3000.0000 | 75714.03 KB |

🔥 *library of this project*

## 2. Update 100.000 entities with two components

Benchmark parameter: Padding = 0

*Notable fact*  
SIMD MonoThread running on a **single core** beats MultiThread running on 8 cores.  
So other threads can still keep running without competing for CPU resources.  

| Method                          |  | Mean        | Error     | StdDev    | Median      | Gen0   | Allocated |
|---------------------------------|--|------------:|----------:|----------:|------------:|-------:|----------:|
| Arch_MonoThread                 |  |    62.29 μs |  0.039 μs |  0.031 μs |    62.29 μs |      - |         - |
| Arch_MultiThread                |  |    48.13 μs |  0.345 μs |  0.322 μs |    48.23 μs |      - |         - |
| DefaultEcs_MonoThread           |  |   125.48 μs |  0.507 μs |  0.450 μs |   125.58 μs |      - |         - |
| DefaultEcs_MultiThread          |  |   127.47 μs |  1.242 μs |  1.101 μs |   127.46 μs |      - |         - |
| FrifloEngineEcs_MonoThread      |🔥|    55.57 μs |  0.699 μs |  0.654 μs |    55.57 μs |      - |         - |
| FrifloEngineEcs_MultiThread     |🔥|    15.96 μs |  0.316 μs |  0.295 μs |    15.94 μs |      - |         - |
| FrifloEngineEcs_SIMD_MonoThread |🔥|    11.94 μs |  0.012 μs |  0.011 μs |    11.94 μs |      - |         - |
| HypEcs_MonoThread               |  |    56.30 μs |  0.050 μs |  0.042 μs |    56.31 μs |      - |     112 B |
| HypEcs_MultiThread              |  |    62.30 μs |  0.031 μs |  0.027 μs |    62.30 μs | 0.2441 |    2081 B |
| LeopotamEcsLite                 |  |   143.43 μs |  0.063 μs |  0.056 μs |   143.45 μs |      - |         - |
| LeopotamEcs                     |  |   136.52 μs |  0.071 μs |  0.066 μs |   136.54 μs |      - |         - |
| MonoGameExtended                |  |   464.74 μs |  0.631 μs |  0.590 μs |   465.01 μs |      - |     161 B |
| Morpeh_Direct                   |  | 1,394.87 μs | 26.879 μs | 27.603 μs | 1,396.43 μs |      - |       2 B |
| Morpeh_Stash                    |  | 1,074.20 μs | 21.396 μs | 58.570 μs | 1,053.22 μs |      - |       2 B |
| RelEcs                          |  |   249.37 μs |  0.882 μs |  0.825 μs |   249.44 μs |      - |     169 B |
| SveltoECS                       |  |   162.80 μs |  0.688 μs |  0.643 μs |   162.45 μs |      - |         - |

🔥 *library of this project*

<br/>


# License

This project is licensed under LGPLv3.  

Friflo.Engine.ECS  
Copyright © 2024   Ullrich Praetz - https://github.com/friflo