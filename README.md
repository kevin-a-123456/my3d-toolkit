# 🎮 My3D Model Tool

**A simple 3D model format that packs geometry + texture into one file.**

---

## 📖 What is My3D?

My3D is a custom 3D model format I designed as a teenager. It stores **vertices, UVs, faces, and textures** in a **single .my3d file** with optional compression. It's simple enough to understand in 10 minutes, but powerful enough for games and 3D applications.

**The goal:** Make 3D model files so simple that anyone can understand them.

---

## ✨ Features

- 📦 **Single file** — Model + texture in one `.my3d` file
- 🗜️ **Compression** — zlib compression saves 40-60% file size
- 🔄 **Convert from OBJ/GLTF/STL** — Drop your models in
- 👀 **Two viewers** — Ursina (desktop) + Web viewer (browser)
- 📖 **Simple format** — Only 32 bytes of header, easy to parse
- 🌍 **Cross-platform** — Windows, macOS, Linux
- 🆓 **Open source** — MIT license

---

## 🚀 Quick Start

### 1. Install dependencies

```bash
pip install pillow numpy trimesh ursina
2. Download the tool
Just download model_tool.py and run it.

3. Convert your first model
bash
python model_tool.py convert model.obj
That's it! You'll get model.my3d.

📖 Usage
Interactive Menu
bash
python model_tool.py
You'll see:

text
╔══════════════════════════════════════════════════════════╗
║   🎮 My3D Model Tool v2.0                              ║
║   OBJ/GLTF/STL → .my3d Converter & Viewer             ║
╚══════════════════════════════════════════════════════════╝

Select an option:
  1. Convert OBJ/GLTF/STL → .my3d
  2. View .my3d model (Ursina)
  3. Generate test cube
  4. Convert with custom texture
  5. Diagnose file
  6. Show help
  0. Exit
Command Line
bash
# Convert model to .my3d
python model_tool.py convert model.obj

# View .my3d file
python model_tool.py view model.my3d

# Generate test cube
python model_tool.py test

# Add texture to model
python model_tool.py addtexture model.obj texture.jpg

# Diagnose .my3d file
python model_tool.py diagnose model.my3d
📁 File Format (.my3d v2)
The format is intentionally simple:

text
┌──────────────────────────────────────────┐
│  HEADER (32 bytes)                      │
│  - Magic: "MY3D"                        │
│  - Version: 2                           │
│  - Vertex count, Face count             │
│  - Texture width, height                │
│  - Flags (compressed or not)            │
├──────────────────────────────────────────┤
│  VERTICES (float32 × V × 3)             │
│  UVS (float32 × V × 2)                  │
│  FACES (uint32 × F × 3)                 │
├──────────────────────────────────────────┤
│  TEXTURE DATA (RGB pixels)              │
│  - Uncompressed: raw RGB bytes          │
│  - Compressed: zlib stream              │
└──────────────────────────────────────────┘
Total header: 32 bytes. That's it!

🖼️ Viewers
Ursina Viewer (Desktop)
bash
python model_tool.py view model.my3d
Controls:

Left drag → Rotate

Scroll → Zoom

Right drag → Pan

ESC → Exit

Web Viewer (Browser)
Open viewer.html in your browser, then drag & drop your .my3d file.

No installation needed!

🛠️ Supported Input Formats
Format	Extension	Texture
Wavefront OBJ	.obj	✅ Auto-extract
GLTF / GLB	.gltf / .glb	✅ Built-in
STL	.stl	❌ (generates checkerboard)
PLY	.ply	⚠️ Vertex colors
Collada DAE	.dae	✅
3MF	.3mf	✅
🗺️ Roadmap
☑ v1: Basic OBJ to .my3d conversion
☑ v2: zlib compression + Ursina viewer
☑ Web viewer (Three.js)
□ v3: Vertex normals (smooth lighting)
□ v3: Multi-material support
□ Blender import/export plugin
□ Unity loader
🤝 Contributing
Found a bug? Have an idea? Feel free to:

Open an Issue

Submit a Pull Request

Star this project ⭐

📄 License
MIT License — Free for personal and commercial use.

🙏 Acknowledgments
Pillow — Image processing

Trimesh — 3D model loading

Ursina — 3D viewer

Three.js — Web viewer

📬 Contact
Open an issue on GitHub or reach out via the discussion board.

Made with ❤️ by a teenage developer

text

---