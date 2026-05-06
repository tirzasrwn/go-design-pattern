**Prototype Factory** - Creates objects based on a prototype/role pattern.

In this code:
- `NewEmployee()` is a factory that creates employees based on role constants
- Uses `iota` for `Developer` and `Manager` roles
- Returns pre-configured prototypes with default position and income
- Client sets only the name: `m := NewEmployee(Manager); m.Name = "Sam"`
- Simple switch-based factory for different employee types

```go
package factories

import "fmt"

type Employee struct {
  Name, Position string
  AnnualIncome int
}

const (
  Developer = iota
  Manager
)

// functional
func NewEmployee(role int) *Employee {
  switch role {
  case Developer:
    return &Employee{"", "Developer", 60000}
  case Manager:
    return &Employee{"", "Manager", 80000}
  default:
    panic("unsupported role")
  }
}

func main() {
  m := NewEmployee(Manager)
  m.Name = "Sam"
  fmt.Println(m)
}
```
