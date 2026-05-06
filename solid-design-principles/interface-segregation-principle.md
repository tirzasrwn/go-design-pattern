**Interface Segregation Principle (ISP)** - Clients shouldn't depend on interfaces they don't use.

In this code:
- `Machine` interface forces `OldFashionedPrinter` to implement `Fax()` and `Scan()` (panics!)
- Violates ISP: printer doesn't need fax/scan, but must implement them
- Solution: Split into `Printer` and `Scanner` interfaces
- `MyPrinter` implements only `Printer` (no fake methods needed)
- `MultiFunctionMachine` combines interfaces via composition (decorator pattern)

```go
package main

type Document struct {

}

type Machine interface {
	Print(d Document)
	Fax(d Document)
	Scan(d Document)
}

// ok if you need a multifunction device
type MultiFunctionPrinter struct {
	// ...
}

func (m MultiFunctionPrinter) Print(d Document) {

}

func (m MultiFunctionPrinter) Fax(d Document) {

}

func (m MultiFunctionPrinter) Scan(d Document) {

}

type OldFashionedPrinter struct {
	// ...
}

func (o OldFashionedPrinter) Print(d Document) {
	// ok
}

func (o OldFashionedPrinter) Fax(d Document) {
	panic("operation not supported")
}

// Deprecated: ...
func (o OldFashionedPrinter) Scan(d Document) {
	panic("operation not supported")
}

// better approach: split into several interfaces
type Printer interface {
	Print(d Document)
}

type Scanner interface {
	Scan(d Document)
}

// printer only
type MyPrinter struct {
	// ...
}

func (m MyPrinter) Print(d Document) {
	// ...
}

// combine interfaces
type Photocopier struct {}

func (p Photocopier) Scan(d Document) {
	//
}

func (p Photocopier) Print(d Document) {
	//
}

type MultiFunctionDevice interface {
	Printer
	Scanner
}

// interface combination + decorator
type MultiFunctionMachine struct {
	printer Printer
	scanner Scanner
}

func (m MultiFunctionMachine) Print(d Document) {
	m.printer.Print(d)
}

func (m MultiFunctionMachine) Scan(d Document) {
	m.scanner.Scan(d)
}

func main() {

}

```
