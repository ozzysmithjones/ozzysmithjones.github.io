
What if I told you that there's a way to completely eliminate a whole category of errors and it doesn't even require rewriting your program in Rust. When I stumbled on this idea I thought "why the hell is this not the standard".

When you think about it, there's only really a few key places where there can be a fatal error in a program, and most fatal errors can be quite easy to resolve. I mean ambiguous logic errors can be harder to find but most fatal issues that programmers actually encounter can be trivial to solve and embarassingly easy to guard against, yet still they cause problems all the time. Many would consider "null" to be the billion dollar mistake - it is trivial to solve most encounters of "null" - the debugger normally points you to the precise place where null was accessed incorrectly - but null reference exceptions happen so frequently that the overall cost to developers time must be somewhere in the billions. Modern languages now solve this problem by introducing optional types, that must be safely unwrapped before the value inside can be used.  I propose that we do a similar thing with indexing.

Indexing is ALSO essentially a fatal problem that is trivial to solve most of the time. An index that is out-bounds can cause memory corruption or a index out of range exception. Most index-out-of-bounds issues can just be a simple off-by-one error. The trick that I have found to solve bounds checking is to make the range that the index can be within part of its type:

```C++
// N and T are generic parameters part of the index type.
// N is the range of the array, the index cannot be more than this value. This is enforced at compile time.
// We can also add some additional type-checking by enforcing that the index can only be used with an array of a certain element type "T"
template<typename T, size_t N>
class bounded_index {
public:
  [[nodiscard]] constexpr size_t get() const {
      return index;
  }
private:
  size_t index;
  friend class array<T, N>;
};
```
Then the API is designed in such a way that the only way that you can source an index is from an iterator or a compile-time expression. In both cases the index can be validated at compile time to be within bounds, so long as the bounds of the array is known ahead of time. 

```C++
template<typename T, size_t N>
class array {
public:
  T& operator[](bounded_index<T, N> index) {
      // no bounds checking needed, the type itself guarantees that this element is within bounds:
      // this is both safer and more efficient than the alternative
      return elements[index.get()];
  }

  template<size_t I>
  [[nodiscard]] constexpr bounded_index<T, N> create_bounded_index() {
      // we can static assert that the index is in bounds at compile time.
      static_assert(I < N, "Cannot produce an index that is out of bounds at compile time."); 
      return bounded_index<T, N>{ I };
  }

  // You can also imagine a full iterator type here that starts at 0 and goes to N, producing bounded indices that are guaranteed to be within bounds at compile time.
private:
  T elements[N];
};
```
I nicked this idea from Ada, which has similar static typing for index types, and I thought that it was a great idea so I decided to share it. I highly recommend Ada it is an interesting language for correctness in general. The only downside with this static typing approach is that you cannot easily compute indices at run-time (apart from indices generated from an iterator), this means you cannot just implement a sorting algorithm with this approach - but programmers generally aren't implementing sorting algorithms very often anyway, and you could just abstract away the sorting with an escape hatch of some kind. In any case the traditional way of indexing could always be a fallback approach.
