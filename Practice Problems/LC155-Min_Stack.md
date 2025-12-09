# Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element val onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with $O(1)$ time complexity for each function.

## Solution

The $O(1)$ requirements for the methods can all be covered by a Stack, except for the `getMin()`. In order to do this, you need to keep track of what the _current minimum_ is along with every push onto thet stack. This way, even if the current minimum is popped out, below that in the stack still tracks the new current minimum.

```java
class MinStack {

    private class Node {
        int val;
        int min;

        Node(int val, int min) {
            this.val = val;
            this.min = min;
        }
    }

    private Deque<Node> stack = new ArrayDeque<>();

    public MinStack() {}

    public void push(int val) {
        if (stack.isEmpty()) {
            stack.push(new Node(val, val));
            return;
        }
        int currentMin = stack.peek().min;
        stack.push(new Node(val, Math.min(currentMin, val)));
    }

    public void pop() {
        stack.pop();
    }

    public int top() {
        return stack.peek().val;
    }

    public int getMin() {
        return stack.peek().min;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

```java
class MinStack {
	private class Node {
		int val;
		int min;
		
		Node(int val, int min) {
			this.val = val;
			this.min = min;
		}
	}
	
	private Deque<Node> stack = new ArrayDeque<>();
	
	public MinStack() {}
	
	public void push(int val) {
		if(stack.isEmpty()) {
			stack.push(new Node(val, val));
			return;
		}
		int currentMin = stack.peek().min;
		stack.push(new Node(val, Math.min(currentMin, val)));
	}
	
	public void pop() {
		stack.push();
	}
	
	public int top() {
		return stack.peek().val;
	}
	
	public int min() {
		return stack.peek().min;
	}
}
```