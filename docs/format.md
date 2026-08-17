
.my3d is a simple binary 3D model format that stores geometry, UV coordinates, faces, and texture data in a single file. It's designed to be easy to parse, debug, and implement.

## File Structure
+------------------+
| HEADER (32 bytes) |
+------------------+
| VERTICES |
| (float32 × N × 3) |
+------------------+
| UVS |
| (float32 × N × 2) |
+------------------+
| FACES |
| (uint32 × M × 3) |
+------------------+
| TEXTURE DATA |
| (RGB or zlib) |
+------------------+

text

## Header (32 bytes)

| Offset | Size | Field | Type | Description |
|--------|------|-------|------|-------------|
| 0 | 4 | MAGIC | char[4] | Must be "MY3D" |
| 4 | 4 | VERSION | uint32 | Must be 2 |
| 8 | 4 | VERT_COUNT | uint32 | Number of vertices |
| 12 | 4 | FACE_COUNT | uint32 | Number of faces |
| 16 | 2 | TEX_W | uint16 | Texture width in pixels |
| 18 | 2 | TEX_H | uint16 | Texture height in pixels |
| 20 | 1 | FLAGS | uint8 | Bit 0: compressed (1=yes, 0=no) |
| 21 | 11 | RESERVED | char[11] | Reserved for future use |

## Geometry Data

### Vertices (VERT_COUNT × 3 floats)

Each vertex is 3 consecutive float32 values: **x, y, z**

### UVs (VERT_COUNT × 2 floats)

Each UV coordinate is 2 consecutive float32 values: **u, v**

### Faces (FACE_COUNT × 3 uint32)

Each face is 3 consecutive uint32 values: **i1, i2, i3** (vertex indices)

## Texture Data

### Uncompressed (FLAGS bit 0 = 0)

Raw RGB pixel data: **TEX_W × TEX_H × 3 bytes**
- Each pixel: 3 bytes (R, G, B)
- Row-major order (left to right, top to bottom)

### Compressed (FLAGS bit 0 = 1)

zlib-compressed RGB pixel data. Decompress to get the raw RGB data described above.

## Example: Test Cube
Vertices: 8
Faces: 12
Texture: 256×256

The cube uses 6 colors, one per face.

text

## Parsing Code (Python)

```python
import struct
import zlib
from PIL import Image

def load_my3d(filepath):
    with open(filepath, 'rb') as f:
        # Read header
        magic = f.read(4)
        version = struct.unpack('I', f.read(4))[0]
        vert_count = struct.unpack('I', f.read(4))[0]
        face_count = struct.unpack('I', f.read(4))[0]
        tex_w = struct.unpack('H', f.read(2))[0]
        tex_h = struct.unpack('H', f.read(2))[0]
        flags = struct.unpack('B', f.read(1))[0]
        f.seek(11, 1)  # reserved

        # Read vertices
        verts = []
        for _ in range(vert_count * 3):
            verts.append(struct.unpack('f', f.read(4))[0])

        # Read UVs
        uvs = []
        for _ in range(vert_count * 2):
            uvs.append(struct.unpack('f', f.read(4))[0])

        # Read faces
        faces = []
        for _ in range(face_count * 3):
            faces.append(struct.unpack('I', f.read(4))[0])

        # Read texture
        tex_data = f.read()
        if flags & 1:  # compressed
            tex_data = zlib.decompress(tex_data)

        texture = Image.frombytes('RGB', (tex_w, tex_h), tex_data)
Version History
Version	Changes
v1	Basic format, no compression
v2	Added zlib compression, 32-byte header
Why so simple?
The design philosophy of .my3d is:

"A 3D format should be understandable in 10 minutes."

No complex nesting. No JSON. No external dependencies. Just a binary file you can read byte by byte.

text
