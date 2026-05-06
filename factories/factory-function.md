**Factory Function** - A simple function that creates and returns objects.

In this code:
- `NewPerson()` is a factory function that creates `Person` objects
- Encapsulates object creation logic
- Can return pointer or value (here returns `*Person`)
- Provides a named constructor alternative to direct struct initialization

```go
package factories

import "fmt"

type Person struct {
  Name string
  Age int
}

// factory function
//func NewPerson(name string, age int) Person {
//  return Person{name, age}
//}

func NewPerson(name string, age int) *Person {
  return &Person{name, age}
}

func main_() {
  // initialize directly
  p := Person{"John", 22}
  fmt.Println(p)

  // use a constructor
  p2 := NewPerson("Jane", 21)
  p2.Age = 30
  fmt.Println(p2)
}
```
