**Prototype Pattern (Serialization)** - Deep copies objects by serializing and deserializing.

In this code:
- `DeepCopy()` uses Go's `encoding/gob` to serialize the object
- Serializes `Person` to bytes, then deserializes to create a clone
- Handles nested structs and slices automatically
- No need to manually copy each field
- Trade-off: slower than manual copy but works generically

```go
package prototype

import (
  "bytes"
  "encoding/gob"
  "fmt"
)

type Address struct {
  StreetAddress, City, Country string
}

type Person struct {
  Name string
  Address *Address
  Friends []string
}

func (p *Person) DeepCopy() *Person {
  // note: no error handling below
  b := bytes.Buffer{}
  e := gob.NewEncoder(&b)
  _ = e.Encode(p)

  // peek into structure
  fmt.Println(string(b.Bytes()))

  d := gob.NewDecoder(&b)
  result := Person{}
  _ = d.Decode(&result)
  return &result
}

func main() {
  john := Person{"John",
    &Address{"123 London Rd", "London", "UK"},
    []string{"Chris", "Matt", "Sam"}}

  jane := john.DeepCopy()
  jane.Name = "Jane"
  jane.Address.StreetAddress = "321 Baker St"
  jane.Friends = append(jane.Friends, "Jill")

  fmt.Println(john, john.Address)
  fmt.Println(jane, jane.Address)

}
```
