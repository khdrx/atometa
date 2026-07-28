# Atometa

**Real-time 3D Medical Education Platform**

Atometa is a native C++ desktop application that lets professors and students share the same 3D anatomical session simultaneously. The professor rotates or zooms a model — every connected student sees the exact same view in real time.

---

## Features

- **Real-time session sync** — WebSocket-based camera sync (professor → students) with sub-10ms latency
- **3D model loading** — GLB / OBJ / FBX / COLLADA via Assimp (40+ formats)
- **Orbital camera** — rotate, zoom, pan with mouse
- **Session management** — host or join a session by IP, no cloud required
- **Scene panel** — load, remove, toggle visibility of models at runtime
- **Properties panel** — position, rotation, scale per model
- **75 FPS** OpenGL renderer with Phong lighting

---

## Tech Stack

| Layer | Library |
|---|---|
| Rendering | OpenGL 3.3 + GLAD + GLFW |
| Math | GLM |
| UI | Dear ImGui |
| Networking | Boost.Beast (WebSocket) + Boost.Asio |
| Model Loading | Assimp |
| Serialization | nlohmann/json |
| Build | CMake 3.20+ + vcpkg |

---

## Project Structure

```
Atometa/
├── assets/
│   ├── models/          ← place .glb / .obj files here
│   ├── shaders/
│   │   ├── basic.vert
│   │   └── basic.frag
│   └── icons/
├── include/
│   └── Atometa/
│       ├── Core/
│       │   ├── Core.h
│       │   ├── Logger.h
│       │   ├── Application.h
│       │   ├── Window.h
│       │   └── Input.h
│       ├── Renderer/
│       │   ├── Camera.h
│       │   ├── Shader.h
│       │   ├── Mesh.h
│       │   ├── Buffer.h
│       │   ├── Renderer.h
│       │   └── ModelLoader.h
│       ├── Scene/
│       │   ├── Scene.h
│       │   └── MedicalModel.h
│       ├── Network/
│       │   └── NetworkLayer.h
│       └── UI/
│           └── ImGuiLayer.h
├── src/
│   ├── core/
│   ├── renderer/
│   ├── scene/
│   ├── network/
│   ├── ui/
│   └── main.cpp
├── CMakeLists.txt
├── vcpkg.json
└── README.md
```

---

## Build (Windows)

### Prerequisites

- Visual Studio 2022 (MSVC 17+)
- CMake 3.20+
- [vcpkg](https://github.com/microsoft/vcpkg) installed and bootstrapped

### Steps

```cmd
# 1 — Clone
git clone https://github.com/YOUR_USERNAME/atometa.git
cd atometa

# 2 — Install dependencies via vcpkg
vcpkg install

# 3 — Configure
cmake -B build -S . ^
  -DCMAKE_TOOLCHAIN_FILE=C:/vcpkg/scripts/buildsystems/vcpkg.cmake ^
  -DVCPKG_TARGET_TRIPLET=x64-windows-static

# 4 — Build
cmake --build build --config Debug
```

The executable is at `build/bin/Atometa.exe`. Assets are copied automatically.

### Release build

```cmd
cmake --build build --config Release
```

---

## Build (Linux / macOS)

> Cross-platform support is in progress (Phase 6).

```bash
# Install system deps (Ubuntu example)
sudo apt install libglfw3-dev libboost-all-dev libassimp-dev

cmake -B build -S .
cmake --build build
```

---

## Usage

### Host a session (Professor)

1. Launch Atometa
2. Load a model: **Scene panel → path field → Load Model**
3. Open **View → Session**
4. Set port (default `8080`) → click **Host Session**
5. Share your IP with students

### Join a session (Student)

1. Launch Atometa
2. Open **View → Session**
3. Enter professor's IP + port → click **Join Session**
4. Camera syncs automatically

### Controls

| Action | Input |
|---|---|
| Rotate | Left mouse drag |
| Pan | Right mouse drag |
| Zoom | Scroll wheel |
| Sensitivity | Slider in menu bar |

---

## Adding Medical Models

Place any `.glb`, `.obj`, or `.fbx` file in `assets/models/`.

Free high-quality heart models:
- [Sketchfab — Realistic Human Heart](https://sketchfab.com) (CC license, GLB download)
- [get3dmodels.com — Detailed Heart](https://get3dmodels.com)
- [3dmodels.org — Heart + Cross Section](https://3dmodels.org) (4K textures)

In-app: **Scene panel → type path → Load Model**

---

## Roadmap

- [x] Phase 1 — Remove Chemistry/Physics/Python
- [x] Phase 2 — Clean Windows build (MSVC)
- [x] Phase 3 — WebSocket real-time camera sync
- [x] Phase 4 — Assimp model loading + Scene/Properties UI
- [ ] Phase 5 — Texture rendering (GLB embedded textures)
- [ ] Phase 6 — Cross-platform Linux/macOS
- [ ] Phase 7 — Customer validation (10 professor interviews)

---

## License

MIT License — see [LICENSE](LICENSE)
