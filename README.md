# Yoctite and Candelite

Geometry and lighting for [Hydroxus](https://github.com/d4140n-4h3-1/Hydroxus), the Fyrox engine
with a Vulkan (wgpu) backend.

| Crate | What it is |
|---|---|
| [`yoctite`](yoctite) | Virtualized, cluster-based geometry: meshes split into clusters, with a DAG of levels of detail. Plain CPU code with no dependencies. |
| [`yoctite-wgpu`](yoctite-wgpu) | Yoctite on the GPU: the geometry pool, culling, the software rasterizer and the G-Buffer pass, on Hydroxus's wgpu backend. |
| [`candelite`](candelite) | Dynamic global illumination after Lumen: a surface cache of cards, traced on Yoctite's geometry, with screen probes, a radiance cache and reflections. |

Games turn them on through Hydroxus's `fyrox-gfx` crate and its `raytracing` feature
(`GraphicsEffects::with_yoctite`, `with_candelite`); they rarely need to depend on these crates
directly. `ROADMAP_GEOMETRY_GI.md` in Hydroxus describes the design.

## Depending on it

The GPU crates take the engine from Hydroxus's `vulkan` branch on GitHub, as the games do, so
Cargo resolves a single engine for everything:

```toml
yoctite = { git = "https://github.com/d4140n-4h3-1/yoctite-candelite.git", branch = "main" }
yoctite-wgpu = { git = "https://github.com/d4140n-4h3-1/yoctite-candelite.git", branch = "main" }
candelite = { git = "https://github.com/d4140n-4h3-1/yoctite-candelite.git", branch = "main" }
```

## Working on it next to a local Hydroxus

To build against a Hydroxus checkout beside this one instead of GitHub, add a git-ignored
`.cargo/config.toml`:

```toml
[patch."https://github.com/d4140n-4h3-1/Hydroxus.git"]
fyrox-core = { path = "../Hydroxus/fyrox-core" }
fyrox-graphics = { path = "../Hydroxus/fyrox-graphics" }
fyrox-graphics-wgpu = { path = "../Hydroxus/fyrox-graphics-wgpu" }
```

The history here was carried over from Hydroxus, where these crates started.

## License

MIT, see [LICENSE.md](LICENSE.md).
