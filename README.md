```markdown
# Feureau's Universal Chromatic Kit

**A fast, cross-platform file viewer with native shell integration.**

Feureau's Universal Chromatic Kit (F.U.C.K.) is a lightweight viewer for SVG,
raster images, and — eventually — a broader range of visual and document
formats. It renders via [SkiaSharp](https://github.com/mono/SkiaSharp) and
[Svg.Skia](https://github.com/wieslawsoltes/Svg.Skia), presents its UI with
[Avalonia](https://avaloniaui.net/), and integrates with the Windows shell
through a native thumbnail and preview handler.

---

## Status

**Early development.** The project is in the scaffolding phase — the solution
structure, core rendering abstractions, and initial UI shell are being laid
down. Nothing here is stable yet. Expect breaking changes.

---

## Features

### Currently in progress
- Cross-platform desktop app (Windows, macOS, Linux)
- SVG rendering via Svg.Skia
- Raster image rendering (PNG, JPEG, WebP, GIF, BMP, TIFF, AVIF) via SkiaSharp
- Tabbed document interface
- Pan and zoom with cursor-anchored scaling
- Transparency checkerboard background

### Planned
- Windows Explorer thumbnail provider (`IThumbnailProvider`)
- Windows Explorer preview pane handler (`IPreviewHandler`)
- Windows right-click context menu integration
- PDF rendering via PDFium
- Markdown and plain-text viewing
- Export to PNG at custom scale and DPI
- Element inspector for SVG files
- Drag-and-drop file opening
- Recent files list
- Animation controls (SMIL, CSS)
- CLI mode for headless batch conversion

---

## Supported formats

| Format | Status |
|---|---|
| SVG / SVGZ | ✅ In progress |
| PNG | ✅ In progress |
| JPEG | ✅ In progress |
| WebP | ✅ In progress |
| GIF | ✅ In progress |
| BMP | ✅ In progress |
| TIFF | ✅ In progress |
| AVIF | ✅ In progress |
| PDF | 🚧 Planned |
| Markdown | 🚧 Planned |
| Plain text | 🚧 Planned |

---

## Building from source

### Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- A C# editor — [Rider](https://www.jetbrains.com/rider/),
  [Visual Studio 2022](https://visualstudio.microsoft.com/), or
  [VS Code](https://code.visualstudio.com/) with the C# Dev Kit
- On Linux, the usual Avalonia native dependencies
  (`libx11`, `libice`, `libsm`, `libfontconfig1`, etc.)

### Build and run

```bash
git clone https://github.com/<your-username>/feureaus-universal-chromatic-kit.git
cd feureaus-universal-chromatic-kit
dotnet restore
dotnet build
dotnet run --project src/ChromaticKit.App
```

### Publish a self-contained build

```bash
# Windows x64
dotnet publish src/ChromaticKit.App -c Release -r win-x64 --self-contained true

# Linux x64
dotnet publish src/ChromaticKit.App -c Release -r linux-x64 --self-contained true

# macOS (Apple Silicon)
dotnet publish src/ChromaticKit.App -c Release -r osx-arm64 --self-contained true
```

---

## Architecture

```
FeureausUniversalChromaticKit.sln
├── src/
│   ├── ChromaticKit.Core/            # Rendering, models, services — no UI deps
│   │   └── Renderers/
│   │       ├── IFileRenderer.cs
│   │       ├── SvgRenderer.cs
│   │       ├── RasterRenderer.cs
│   │       └── RendererFactory.cs
│   ├── ChromaticKit.App/             # Avalonia cross-platform UI
│   └── ChromaticKit.Windows.Shell/   # Windows COM shell extension
```

The **Core** library has no UI dependencies and is shared between the
desktop app and the Windows shell extension. The **App** is the
cross-platform viewer. The **Windows.Shell** project produces the COM DLL
that registers the thumbnail and preview handlers with Windows Explorer.

---

## Design notes

- **Rendering** happens through a single `IFileRenderer` interface. New
  formats are added by implementing the interface and registering the
  renderer in `RendererFactory`. No changes to the UI are required.
- **Shell integration** is a separate binary from the main app. Explorer
  loads a small COM DLL, which calls into `ChromaticKit.Core` to generate
  the thumbnail or preview. This keeps the Explorer process lightweight and
  crash-isolated from the full app.
- **Safe-by-default SVG rendering.** Scripts embedded in SVG files are never
  executed. The app treats SVG as untrusted content, consistent with how
  browsers handle `<img src="file.svg">`.

---

## Roadmap

- [x] Project scaffolding and branding
- [ ] Core rendering pipeline (SVG + raster)
- [ ] Avalonia UI shell with file open and viewport
- [ ] Pan, zoom, and checkerboard background
- [ ] Tabbed documents
- [ ] Recent files and settings persistence
- [ ] Windows shell extension — thumbnails
- [ ] Windows shell extension — preview handler
- [ ] PNG export at custom scale
- [ ] PDF rendering
- [ ] Text and Markdown rendering
- [ ] Element inspector for SVG
- [ ] Animation controls
- [ ] CLI mode

---

## Contributing

Contributions are welcome. The project is small enough that there's no
formal process yet — open an issue to discuss what you'd like to work on,
or send a pull request directly. Please keep changes focused and consistent
with the existing architecture.

---

## License

Licensed under the **GNU General Public License v3.0**. See [LICENSE](LICENSE)
for the full text.

This means you're free to use, study, modify, and redistribute the software,
provided that derivative works are also released under the GPLv3.

---

## Acknowledgements

Built on the shoulders of:

- [Avalonia](https://avaloniaui.net/) — cross-platform .NET UI framework
- [SkiaSharp](https://github.com/mono/SkiaSharp) — 2D graphics engine
- [Svg.Skia](https://github.com/wieslawsoltes/Svg.Skia) — SVG rendering for Skia
- [CommunityToolkit.Mvvm](https://github.com/CommunityToolkit/dotnet) — MVVM helpers
- [SharpShell](https://github.com/dwmkerr/sharpshell) — Windows shell extension framework

And yes, the initials spell out what you think they spell out. We know. So don't bother pointing that out.
```
