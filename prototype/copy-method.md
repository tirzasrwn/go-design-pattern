**Prototype Pattern (Copy Method)** - Creates new objects by cloning existing ones via a copy method.

In this code:
- `Address` and `Person` have `DeepCopy()` methods
- `DeepCopy()` creates a completely independent copy (not shallow)
- Copies nested structs (`Address`) and slices (`Friends`) properly
- `jane := john.DeepCopy()` creates clone without affecting `john`
- Changes to `jane` don't affect `john`

```go
package prototype

import "fmt"

type Address struct {
  StreetAddress, City, Country string
}

func (a *Address) DeepCopy() *Address {
  return &Address{
    a.StreetAddress,
    a.City,
    a.Country }
}

type Person struct {
  Name string
  Address *Address
  Friends []string
}

func (p *Person) DeepCopy() *Person {
  q := *p // copies Name
  q.Address = p.Address.DeepCopy()
  copy(q.Friends, p.Friends)
  return &q
}

func main() {
  john := Person{"John",
    &Address{"123 London Rd", "London", "UK"},
    []string{"Chris", "Matt"}}

  jane := john.DeepCopy()
  jane.Name = "Jane"
  jane.Address.StreetAddress = "321 Baker St"
  jane.Friends = append(jane.Friends, "Angela")

  fmt.Println(john, john.Address)
  fmt.Println(jane, jane.Address)
}
```
