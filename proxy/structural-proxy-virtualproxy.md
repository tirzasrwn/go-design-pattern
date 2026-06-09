**Proxy Pattern (Virtual Proxy)** - Delays object creation until it's actually needed (lazy loading).

In this code:
- `Bitmap` loads the image immediately in `NewBitmap()` (eager loading)
- `LazyBitmap` defers loading: image file is only loaded when `Draw()` is first called
- `LazyBitmap` checks if `bitmap == nil`; if so, creates and loads `Bitmap` lazily
- Subsequent calls to `Draw()` reuse the already-loaded bitmap
- Saves resources if the image is never actually drawn

```go
package main

import "fmt"

type Image interface {
  Draw()
}

type Bitmap struct {
  filename string
}

func (b *Bitmap) Draw() {
  fmt.Println("Drawing image", b.filename)
}

func NewBitmap(filename string) *Bitmap {
  fmt.Println("Loading image from", filename)
  return &Bitmap{filename: filename}
}

func DrawImage(image Image) {
  fmt.Println("About to draw the image")
  image.Draw()
  fmt.Println("Done drawing the image")
}

type LazyBitmap struct {
  filename string
  bitmap *Bitmap
}

func (l *LazyBitmap) Draw() {
  if l.bitmap == nil {
    l.bitmap = NewBitmap(l.filename)
  }
  l.bitmap.Draw()
}

func NewLazyBitmap(filename string) *LazyBitmap {
  return &LazyBitmap{filename: filename}
}

func main() {
  //bmp := NewBitmap("demo.png")
  bmp := NewLazyBitmap("demo.png")
  DrawImage(bmp)
}
```

