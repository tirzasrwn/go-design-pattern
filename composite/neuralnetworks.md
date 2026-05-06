**Composite Pattern** - Treats individual objects and compositions uniformly.

In this code:
- `NeuronInterface` defines `Iter()` for both single neurons and layers
- `Neuron` is a leaf node (returns itself in `Iter()`)
- `NeuronLayer` is a composite (returns all neurons in `Iter()`)
- `Connect()` works with any `NeuronInterface` - single neuron or layer
- Client can connect: neuron-to-neuron, neuron-to-layer, layer-to-layer

```go
package composite

type NeuronInterface interface {
  Iter() []*Neuron
}

type Neuron struct {
  In, Out []*Neuron
}

func (n *Neuron) Iter() []*Neuron {
  return []*Neuron{n}
}

func (n *Neuron) ConnectTo(other *Neuron) {
  n.Out = append(n.Out, other)
  other.In = append(other.In, n)
}

type NeuronLayer struct {
  Neurons []Neuron
}

func (n *NeuronLayer) Iter() []*Neuron {
  result := make([]*Neuron, 0)
  for i := range n.Neurons {
    result = append(result, &n.Neurons[i])
  }
  return result
}

func NewNeuronLayer(count int) *NeuronLayer {
  return &NeuronLayer{make([]Neuron, count)}
}

func Connect(left, right NeuronInterface) {
  for _, l := range left.Iter() {
    for _, r := range right.Iter() {
      l.ConnectTo(r)
    }
  }
}

func main() {
  neuron1, neuron2 := &Neuron{}, &Neuron{}
  layer1, layer2 := NewNeuronLayer(3), NewNeuronLayer(4)

  //neuron1.ConnectTo(&neuron2)

  Connect(neuron1, neuron2)
  Connect(neuron1, layer1)
  Connect(layer2, neuron1)
  Connect(layer1, layer2)
}
```
