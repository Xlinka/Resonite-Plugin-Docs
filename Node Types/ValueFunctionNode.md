# ValueFunctionNode Documentation

`ValueFunctionNode` is a specialized abstract class in the ProtoFlux framework designed to evaluate and produce a single output value based on a given execution context. It's part of the `ProtoFlux.Runtimes.Execution` namespace.

## Key Components

- `OwnerNode`: Property returning the current node instance.
- `OutputCount`: Property indicating the number of outputs, which is 1 for this node type.
- `OutputType`: Property indicating the type of the output value.
- `OutputDataClass`: Property indicating the data class of the output.
- `CanBeEvaluated`: Property indicating whether the node can be evaluated, returns `true` by default.
- `GetOutput(int index)`, `GetOutputType(int index)`, and `GetOutputTypeClass(int index)`: Methods for retrieving output-related information based on the index. An `ArgumentOutOfRangeException` is thrown if the index is not 0.
- `Evaluate(C context)`: Sealed method evaluating the node, computing the output value, and pushing it to the context.
- `Compute(C context)`: Abstract method to be implemented by derived classes to define the logic for computing the output value.

## Example Usage

Below is an example of how to create a custom node by extending `ValueFunctionNode`:

```csharp
using ProtoFlux.Core;
using ProtoFlux.Runtimes.Execution;

public class SquareNode : ValueFunctionNode<ExecutionContext, float>
{
    public ValueArgument<float> Input;

    protected override float Compute(ExecutionContext context)
    {
        float inputVal = Input.ReadValue(context);
        return inputVal * inputVal;  // Computes square of the input
    }

    public SquareNode()
    {
        Input = new ValueArgument<float>(this);
    }
}
```

In this example, a SquareNode is created that computes the square of a given input value. The Compute method is overridden to define the logic for computing the square, and the Input argument is defined to receive the input value.