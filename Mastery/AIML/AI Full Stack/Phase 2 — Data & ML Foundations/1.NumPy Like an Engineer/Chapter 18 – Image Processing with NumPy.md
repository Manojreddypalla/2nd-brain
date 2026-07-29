# Mental Model

An image is just a matrix.

Grayscale

```
0   50  120

80  255 60

20  40  90
```

Each number = Pixel brightness

```
0

↓

Black

----------------

255

↓

White
```

---

# Create an Image

```python
import numpy as np

image = np.random.randint(
    0,
    256,
    (5,5),
    dtype=np.uint8
)

print(image)
```

Example

```python
[[120  55 220  14 200]
 [ 60 180  90 255  10]
 [ 30  45 100 210  80]
 [170  20 240 130  75]
 [255 110  40  90 160]]
```

---

# Crop

Take the center

```python
crop = image[1:4,1:4]
```

Result

```
180  90 255

45 100 210

20 240 130
```

---

# Rotate

```python
np.rot90(image)
```

Rotate

90°

---

# Flip Horizontal

```python
np.fliplr(image)
```

---

# Flip Vertical

```python
np.flipud(image)
```

---

# Increase Brightness

```python
bright = np.clip(image+50,0,255)
```

---

# Decrease Brightness

```python
dark = np.clip(image-50,0,255)
```

---

# Negative Image

```python
255-image
```

White becomes black.

Black becomes white.

---

# Threshold

```python
image[image<100]=0
```

Dark pixels

↓

Black

---

```python
image[image>=100]=255
```

Bright pixels

↓

White

---

# RGB Image

Shape

```python
(H,W,3)
```

Example

```python
(720,1280,3)
```

```
Height

Width

RGB
```

---

Access Pixel

```python
image[100,200]
```

Output

```python
[255,120,80]
```

Meaning

```
Red

Green

Blue
```

---

Only Red

```python
image[:,:,0]
```

---

Only Green

```python
image[:,:,1]
```

---

Only Blue

```python
image[:,:,2]
```

---

# Cheat Sheet

| Operation | Function |
|----------|----------|
| Crop | `image[rows,cols]` |
| Rotate | `np.rot90()` |
| Flip Left/Right | `np.fliplr()` |
| Flip Up/Down | `np.flipud()` |
| Brightness | `np.clip(image+50)` |
| Negative | `255-image` |
| Threshold | `image[image<100]=0` |

---

# Practice

Create

```python
image=np.random.randint(
0,
256,
(8,8),
dtype=np.uint8
)
```

Try

```python
np.rot90(image)

np.fliplr(image)

np.flipud(image)

255-image

np.clip(image+40,0,255)

image[2:6,2:6]
```

---

# Key Takeaways

- Images are NumPy arrays.
- Every pixel is just a number.
- RGB images have 3 channels.
- Cropping is slicing.
- Brightness is simple addition.
- Computer vision starts with NumPy.