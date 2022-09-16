# Deducing This

- [Gist](#gist)
- [Duplicated Overloads](#duplicated-overloads)
- [CRTP](#crtp)
    - [Example - Builder Pattern](#example---builder-pattern)
- [Recursive Lambda](#recursive-lambda)
- [Accessing Class Members](#accessing-class-members)
- [Resources](#resources)

## Gist
Ever had to copy and paste a member function multiple times because of all the `const`/`lvalue`/`rvalue` overloads? Why not let the compiler generate the overloads?

## Duplicated Overloads
Often times there might be multiple overloads to a function, where each version might have the same code:
```cpp
class foo {
    auto bar() & { ... }
    auto bar() && { ... }
    auto bar() const & { ... }
    auto bar() const && { ... }
};
```

Using deducing this, this issue can be solved:
```cpp
class foo {
    template <class Self>
    auto bar(this Self&& self) { ... }
};
```
The compiler will generate the overloads for us when they are used. The compiler will substitute `Self` with the correct `this`, retaining all the CV qualifiers and `lvalue`/`rvalue`.

## CRTP
When using deducing this, the `Self` template will always be deduced to the most derived type. This means that on a derived object, the deduced `Self` on the base object will be the derived type. Without deducing this, if we want to call a function on the derived type from base, we would need to cast it to the derived type first (effectively CRTP):
```cpp
template <class T>
struct base {
    void base_func() {
        // cast to derived to call `derived_func`
        auto derived = static_cast<T&>(*this);
        derived.derived_func();
    }
};

struct derived : public base<derived> {
    void derived_func() { ... }
};
```

Using deducing this, the code can be simplified:
```cpp
struct base {
    template <class Self>
    void base_func(this Self&& self) {
        self.derived_func();
    }
};

struct derived : public base {
    void derived_func() { ... }
};
```
No casts involved since when calling `base_func` from a derived type, `Self` will be deduced to the derived type. Also notice that the 2 `struct`s are not template anymore.

### Example - Builder Pattern
Without using deducing this, a typical builder pattern (using CRTP) will look like this:
```cpp
template <class T = void>
class builder {
public:
    // derived type
    using Derived = std::conditional_t<std::is_void_v<T>, builder, T>;

    Derived& a() { ...; return self(); }
    Derived& b() { ...; return self(); }

private:
    // gets derived object
    Derived& self() { return *(static_cast<Derived *>(this)); }
};

class specific_builder : public builder<specific_builder> {
public:
    specific_builder& c() { ...; return *this; }
    specific_builder& d() { ...; return *this; }
};
```

We will need the `Derived` type in base `builder` to allow any derived type to chain their methods, allowing code like `specific_builder{}.a().b().c().d()`. Using deducing this, we can simplify this code:
```cpp
class builder {
public:
    template <class Self>
    Self& a(this Self&& self) { ...; return self; }

    template <class Self>
    Self& b(this Self&& self) { ...; return self; }
};

class specific_builder : public builder {
public:
    template <class Self>
    Self& c(this Self&& self) { ...; return self; }

    template <class Self>
    Self& d(this Self&& self) { ...; return self; }
};
```

## Recursive Lambda
Deducing this can also be used on lambdas. For example:
```cpp
auto fib = [](this auto self, int n) {
    return n < 2 ? n : self(n - 1) + self(n - 2);
}
```
In this case `self` is deduced to lambda's closure object, and passed by value since the closure object generated for `fib` is small.

## Accessing Class Members
Deducing this functions are more like free functions, you cannot directly access `this` pointer. When inside a deducing this function, use the parameter instead:
```cpp
struct foo {
    int num = 0;

    void bar(this auto&& self) {
        self.num = 1;   // OK
        this->num = 1;  // error
    }
};
```

## Resources
| What          | URL                                                                   |
| :------------ | :-------------------------------------------------------------------- |
| Deducing This | https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2021/p0847r6.html |
| CppCon 2021   | https://www.youtube.com/watch?v=jXf--bazhJw                           |
