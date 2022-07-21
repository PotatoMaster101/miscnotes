# Ranges

- [Gist](#gist)
- [Using Ranges](#using-ranges)
    - [Example - Sorting](#example---sorting)
- [Chaining Ranges](#chaining-ranges)
    - [Example - Remove Starting Elements by Predicate](#example---remove-starting-elements-by-predicate)
    - [Example - Filter Even Numbers](#example---filter-even-numbers)
    - [Example - Multiply Even Numbers By 2](#example---multiply-even-numbers-by-2)
- [Refinements](#refinements)
- [Resources](#resources)

## Gist
We want to use containers as a whole rather than using iterators. Wouldn't it be nice to have `std::sort(vec)` rather than `std::sort(vec.begin(), vec.end())`?

## Using Ranges
Ranges are introduced in C++20, lives in the header `<ranges>`. Most of the STL algorithms can be used with ranges.

### Example - Sorting
The old way:
```cpp
std::vector<int> nums{5, 4, 3, 2, 1};
std::sort(nums.begin(), nums.end());
```

The ranges way:
```cpp
std::vector<int> nums{5, 4, 3, 2, 1};
auto sorted = std::ranges::sort(nums);
```

## Chaining Ranges
Ranges can be chained using the `|` operator, similar to CLI pipelines.

### Example - Remove Starting Elements by Predicate
Use `drop_while` to drop N numbers from the start of the collection until the predicate becomes false.
```cpp
std::vector<int> nums{5, 4, 3, 2, 1};

// [3, 2, 1]
auto dropped = nums | std::ranges::views::drop_while([](int n){ return n > 3; });
```

### Example - Filter Even Numbers
Use `filter` to filter out even numbers.
```cpp
std::vector<int> nums{5, 4, 3, 2, 1};

// [4, 2]
auto even = nums | std::ranges::views::filter([](int n){ return n % 2 == 0; });
```

### Example - Multiply Even Numbers By 2
Use `filter` to filter out even numbers then use `transform` to perform multiplication.
```cpp
std::vector<int> nums{5, 4, 3, 2, 1};

// [8, 4]
auto even = nums | std::ranges::views::filter([](int n){ return n % 2 == 0; })
                 | std::ranges::views::transform([](int n){ return n * 2; });
```

## Refinements
There are a few refinements on `std::ranges::range`:
- `std::ranges::input_range`: the iterator type is `std::input_iterator`
- `std::ranges::output_range`: the iterator type is `std::output_iterator`
- `std::ranges::forward_range`: the iterator type is `std::forward_iterator`, such as `std::forward_list`
- `std::ranges::bidirectional_range`: the iterator type is `std::bidirectional_iterator`, such as `std::list`
- `std::ranges::random_access_range`: the iterator type is `std::random_access_iterator`, such as `std::vector`
- `std::ranges::contiguous_range`: the iterator type is `std::contiguous_iterator`
- `std::ranges::common_range`: the `begin` and `end` returns the same type `T`. Currently all standard containers satisfy this `concept`
- `std::ranges::viewable_range`: the range can be safely converted to a `std::ranges::view`, such as `std::vector`

## Resources
| What           | URL                                      |
| :------------- | :--------------------------------------- |
| Ranges Library | https://en.cppreference.com/w/cpp/ranges |
