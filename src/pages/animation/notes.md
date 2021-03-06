## Hints for Implementing the Assignment

In our meeting we agreed on the following interface:

```scala
trait EventStream[A] {
  def map[B](f: A => B): EventStream[B]

  def foldp[B](seed: B)(f: (A, B) => B): EventStream[B]

  def join[B, C](that: EventStream[B])(f: (A, B) => C): EventStream[C] 
}
```

We discussed two general models for implementing reactive systems: push and pull based evaluation. You can implement either, but I'm going to describe a push based model.

In a push model, evaluation starts at a source, when an event arrives, and propagates out to connected nodes. Pull based is the opposite---it starts from sinks and propogates back to sources, polling for changes.

We'll call the direction from source to sink in the graph "forward". Nodes that are forward and connected to a given node are "listeners".

For every node, a pull based model must known the listeners of that node. When a node receives a change we should perform any actions associated with that node and then push the update to its listeners.

(Is the order in which we push updates---for example, depth- or breadth-first---important?)


### Map

Branch your work of the `atoms-and-operations` branch. (If you are feeling adventurous, try the `feature/keydown` branch, which adds a callback for key presses.)

Let's start by just implementing `map`. Unlike with, say, a `List`, we can't actually do the `map` at the point it is called, as the data is not available yet. We can use the same trick we use with `Image`: represent the computation as data.

Implement `map`.

(Did you find you had to implement a private interface to push updates? Can you pull this out o the public interface?)

### Animations

Now implement a method that, given a `Canvas`, converts the `setAnimationFrameCallback` to an `EventStream`, with one event per call for a new frame. (Where is a good place for this method to live?)

Finally implement a method (where?) that, given a `Canvas` and an `EventStream[Image]` animates the `Image` on the `Canvas`. It's sufficient to, for each frame, `clear` the `Canvas` and then `draw` the `Image`.

### Fold

Now implement `foldp` (or just `fold`) using the same technique you used to implement `map`.

### Join

Implementing `join` is probably the trickiest aspect of this section. The intention with `join` is to combine the most recent values from each `EventStream`. A `join` node is therefore a listener to two nodes. We will need store the most recent value from each stream we're listening to, and also represent the possibility there is no most recent value.

### Extensions

We've now built a very simple reactive system. There are lots of further areas to ponder. Here are a few.

#### Error Handling

It's possible that nodes could fail. How should we handle this?

#### Finite EventStreams

Similarly to failure, a node may not be capable of producing any more values (think of representing, say, a web request as an `EventStream`). Should we represent this?

#### Resource Collection

How can be clean up nodes that are no longer needed, so we don't leak memory?

#### Monads

`flatMap` is notably absent from our `EventStream` API. What would is mean to add `flatMap`? 

#### Evaluation Semantics

We briefly pondered the difference between depth- and breadth-first evaluation above. What about concurrency? The reactive model is a natural fit for concurrent processing. Can we extend it to support concurrency? How should we do this? While we're thinking about concurrency, are there race conditions in the existing implementation?

To get you started, consider the following network. What support it evaluation to? What does it evaluate to?

~~~ scala
val a = EventStream.zeros // 0 0 0 0 ....
val x = a map (_+1) foldp(0)(_+_) // 1 2 3 ...
val y = a map (_-1) foldp(0)(_+_) // -1 -2 -3 ...
val z = (x zip y) map (case (x, y) => x + y) // 0 0 0 ... ?
~~~

Should nodes start processing events as soon as they are created, or should they wait for some signal to start? What difference does it make? 
