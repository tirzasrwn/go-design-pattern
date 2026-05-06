**Builder Pattern (Functional)** - Uses functions as building blocks stored in a slice.

In this code:
- `Person` is the object being built
- `personMod` is a function type that modifies a Person
- `PersonBuilder` stores a slice of `personMod` functions
- `Build()` applies all stored modifications to create the final object
- Easy to extend: just add more modifier functions

```go
package main

import "fmt"

type Person struct {
  name, position string
}

type personMod func(*Person)
type PersonBuilder struct {
  actions []personMod
}

func (b *PersonBuilder) Called(name string) *PersonBuilder {
  b.actions = append(b.actions, func(p *Person) {
    p.name = name
  })
  return b
}

func (b *PersonBuilder) Build() *Person {
  p := Person{}
  for _, a := range b.actions {
    a(&p)
  }
  return &p
}

// extend PersonBuilder
func (b *PersonBuilder) WorksAsA(position string) *PersonBuilder {
  b.actions = append(b.actions, func(p *Person) {
    p.position = position
  })
  return b
}

func main() {
  b := PersonBuilder{}
  p := b.Called("Dmitri").WorksAsA("dev").Build()
  fmt.Println(*p)
}
```
