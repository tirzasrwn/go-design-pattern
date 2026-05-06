**Prototype Pattern (Manual Deep Copy)** - Demonstrates the need for deep copying.

In this code:
- Shows the problem with shallow copy: `jane := john` shares the same `Address` pointer
- Changing `jane.Address.StreetAddress` also changes `john.Address` (bug!)
- Solution: manually create new `Address` with copied values
- `jane.Address = &Address{john.Address.StreetAddress, ...}` creates independent copy

```go
package prototype

import "fmt"

type Address struct {
  StreetAddress, City, Country string
}

type Person struct {
  Name string
  Address *Address
}

func main() {
  john := Person{"John",
    &Address{"123 London Rd", "London", "UK"}}

  //jane := john
  // shallow copy
  //jane.Name = "Jane" // ok
  // john address got changed too
  //jane.Address.StreetAddress = "321 Baker St" // nok
  //fmt.Println(john.Name, john.Address)
  //fmt.Println(jane.Name, jane. Address)

  // what you really want
  jane := john
  jane.Address = &Address{
    john.Address.StreetAddress,
    john.Address.City,
    john.Address.Country  }

  jane.Name = "Jane" // ok

  jane.Address.StreetAddress = "321 Baker St"

  fmt.Println(john.Name, john.Address)
  fmt.Println(jane.Name, jane. Address)
}
```
