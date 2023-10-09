# VoidNode Documentation

`VoidNode` is a core abstract class in the ProtoFlux framework, designed to represent a type of execution node. It's part of the `ProtoFlux.Runtimes.Execution` namespace and extends `ExecutionNode<C>`, where `C` is a generic type parameter derived from `ExecutionContext`. The purpose of `VoidNode` is to provide a structured way to compute outputs based on provided inputs, without returning any value.

## Structure

```csharp
using System;
using ProtoFlux.Runtimes.Execution;

public abstract class VoidNode<C> : ExecutionNode<C> where C : ExecutionContext
{
	public override bool CanBeEvaluated => true;

	public override void Evaluate(C context)
	{
		ComputeOutputs(context);
		context.PopInputs();
	}

	protected virtual void ComputeOutputs(C context)
	{
		throw new NotImplementedException($"Method Compute() is not implemented on derived type {GetType()}");
	}
}
```

## Key Components
- `CanBeEvaluated`: A property that indicates whether the node can be evaluated. It returns `true` by default.
- `Evaluate(C context)`: A method that is called to evaluate the node. It, in turn, calls `ComputeOutputs(context)` to compute the outputs, and `context.PopInputs()` to clear the inputs after computation.
- `ComputeOutputs(C context)`: A virtual method intended to be overridden by derived classes to implement the logic for computing outputs.

## Example Usage
Here's an example of how to create a custom node by extending `VoidNode`:

```csharp
using ProtoFlux.Core;
using ProtoFlux.Runtimes.Execution;

[NodeCategory("Utility/Binary")]
public class Adder : VoidNode<ExecutionContext>
{
	public ValueArgument<bool> A;
	public ValueArgument<bool> B;
	public readonly ValueOutput<bool> Sum;

	protected override void ComputeOutputs(ExecutionContext context)
	{
		bool a = A.ReadValue(context);
		bool b = B.ReadValue(context);
		Sum.Write(a ^ b, context);  // XOR operation to compute sum
	}

	public Adder()
	{
		Sum = new ValueOutput<bool>(this);
	}
}

```