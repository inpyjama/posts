+++
date  = "2023-07-14"
title = 'What are Ranges in C++20 ?'

author = "Wasim Akram"
authorImage ="/teams/wasim.jpg"
preferred = "https://www.linkedin.com/in/wasim-akram-6a86a09b/"
linkedin = "https://www.linkedin.com/in/wasim-akram-6a86a09b/"
twitter = ""
blog = ""
email = "wasim@inpyjama.com"

tags = [
    "c++ 20",
]

categories = [
    "c++",
]

series = ["C++"]
images = ["/post/ranges-cpp20/1.webp"]
+++

C++20 introduced Ranges, a powerful standard library feature that simplifies code by replacing loops and iterators with a functional programming style for working with sequences.
<!--more-->

C++20 introduced a new standard library feature called Ranges, which provides a powerful and expressive way to work with sequences of elements. Ranges simplify the code by replacing traditional loops and iterators with a functional programming style. They offer a wide range of operations for manipulating and transforming sequences, making code more concise, readable, and expressive.

## Traditional Iterators vs. Ranges

In pre-C++20, iterating over a container or a sequence of elements involved using iterators and manually writing loops. This approach often led to verbose code, with iterators requiring explicit management and complex loop structures. Ranges aim to simplify this process by providing a higher-level abstraction for working with sequences.

Ranges allow you to express operations directly on sequences, without the need for explicit loops and iterator manipulation. They provide a unified and consistent syntax for a variety of operations, such as filtering, transforming, slicing, sorting, and more. Ranges operate lazily, meaning that they evaluate only as much as needed, improving performance by avoiding unnecessary computations.

## Key Concepts in Ranges
To understand ranges in C++20, let's explore some key concepts:

1. **Range**: A range represents a sequence of elements. It can be any container, array, or even an infinite sequence. Ranges have a common set of operations that can be applied, making them highly composable.
1. **View**: A view is an object that provides a read-only representation of a range. Views are lightweight and do not modify the underlying data. They allow for chaining multiple operations together to form a pipeline of transformations on a range.
1. **Algorithm**: An algorithm is a function that operates on a range or a view. Algorithms perform operations such as sorting, searching, filtering, and transforming the elements of a range. They take one or more ranges/views as input and produce a result or perform side effects.
1. **Pipe Operator (`|`)**: The pipe operator (`|`) allows you to chain views and algorithms together, creating a concise and expressive syntax. It passes the result of the left-hand side as input to the right-hand side.

## Example Usage of Ranges
To demonstrate the power and simplicity of ranges, let's look at an example:

```cpp {title="ranges.cpp"}
#include <iostream>
#include <vector>
#include <ranges>

int main() {
  std::vector<int> numbers{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

  auto evenNumbers = numbers | std::views::filter([](int x) { return x % 2 == 0; });

  for (int x : evenNumbers) {
    std::cout << x << " ";
  }
  std::cout << std::endl;

  return 0;
}
```

In this code, we start with a vector of numbers and use the filter view from the `std::views` namespace to create a new view that only includes even numbers. We then iterate over this view using a range-based `for` loop and print each element.

This concise syntax eliminates the need for manual loops and if statements, making the code more readable and expressive.

To compile this program, you would use the following command:

```bash {title="compile ranges.cpp" }
g++ -std=c++20 ranges.cpp -o ranges
```

## Benefits of Ranges
Ranges offer several benefits over traditional iterator-based approaches:

1. Improved Readability: Ranges provide a higher-level abstraction, resulting in more readable and expressive code. The operations can be chained together, making the code read like a series of transformations applied to a range.
1. Code Reusability: With ranges, you can create reusable views and algorithms that can be applied to different ranges or views. This promotes code reuse and modular design.
1. Lazy Evaluation: Ranges are evaluated lazily, meaning that computations are performed only when required. This can improve performance by avoiding unnecessary calculations.
1. Enhanced Expressiveness: Ranges enable a more functional programming style in C++. You can leverage lambdas and other functional constructs to express complex transformations succinctly.

## Examples

Here are the code examples that demonstrate the usage of ranges in C++20, along with their corresponding outputs:

1. Filtering even numbers:
    ```cpp {title="ranges.cpp"}
    #include <iostream>
    #include <vector>
    #include <ranges>

    int main() {
      std::vector<int> numbers{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

      auto evenNumbers = numbers | std::views::filter([](int x) { return x % 2 == 0; });

      for (int x : evenNumbers) {
        std::cout << x << " ";
      }
      std::cout << std::endl;

      return 0;
    }
    ```
    ```bash {title="output"}
    2 4 6 8 10
    ```
1. Transforming strings to uppercase:
    ```cpp {title="ranges.cpp"}
    #include <iostream>
    #include <string>
    #include <ranges>

    int main() {
      std::string text = "hello world";

      auto uppercaseText = text | std::views::transform([](char c) { return std::toupper(c); });

      for (char c : uppercaseText) {
        std::cout << c;
      }
      std::cout << std::endl;

      return 0;
    }
    ```
    ```bash {title="output"}
    HELLO WORLD
    ```
1. Searching for specific elements:
    ```cpp {title="ranges.cpp"}
    #include <iostream>
    #include <vector>
    #include <ranges>
    #include <algorithm>

    int main() {
      std::vector<int> numbers{1, 2, 3, 4, 5};

      auto it = std::ranges::find(numbers, 3);
      if (it != numbers.end()) {
        std::cout << "Found 3 in the vector" << std::endl;
      }

      return 0;
    }
    ```
    ```bash {title="output"}
    Found 3 in the vector
    ```

1. Processing file contents:
    ```cpp {title="ranges.cpp"}
    #include <iostream>
    #include <fstream>
    #include <string>
    #include <ranges>

    int main() {
      std::ifstream file("data.txt");
      if (!file) {
        std::cerr << "Failed to open file." << std::endl;
        return 1;
      }

      auto lines = std::ranges::istream_view<std::string>(file);

      for (const std::string& line : lines) {
        std::cout << line << std::endl;
      }

      return 0;
    }
    ```
    ```bash {title="output"}
    Contents of the file data.txt
    ```
1. Filtering and processing network packets:
    ```cpp {title="ranges.cpp"}
    #include <iostream>
    #include <vector>
    #include <ranges>
    #include <algorithm>

    struct Packet {
      std::string sourceIP;
      std::string destinationIP;
      int size;
    };

    int main() {
      std::vector<Packet> packets{
        {"192.168.0.1", "192.168.0.2", 100},
        {"192.168.0.3", "192.168.0.4", 200},
        {"192.168.0.5", "192.168.0.6", 150},
        {"192.168.0.7", "192.168.0.8", 120}
      };

      auto largePackets = packets | std::views::filter([](const Packet& packet) { return packet.size >= 150; });

      for (const Packet& packet : largePackets) {
        std::cout << "Source: " << packet.sourceIP << ", Destination: " << packet.destinationIP << ", Size: " << packet.size << std::endl;
      }

      return 0;
    }
    ```
    ```bash {title="output"}
    Source: 192.168.0.3, Destination: 192.168.0.4, Size: 200
    Source: 192.168.0.5, Destination: 192.168.0.6, Size: 150
    ```

## More snippets

These examples provide further demonstrations of working with ranges in C++20 and their expected outputs.

1. Printing elements of a vector:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};

    std::ranges::for_each(numbers, [](int n){ std::cout << n << ' '; });

    // Output: 1 2 3 4 5
    ```
1. Transforming a range by squaring each element:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};

    auto squared_numbers = numbers | std::views::transform([](int n){ return n * n; });

    for (int n : squared_numbers) {
        std::cout << n << ' ';
    }

    // Output: 1 4 9 16 25
    ```
1. Checking if all elements in a range satisfy a condition:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};

    bool all_positive = std::ranges::all_of(numbers, [](int n){ return n > 0; });

    std::cout << std::boolalpha << all_positive;

    // Output: true
    ```
1. Counting the occurrences of a value in a range:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 2, 3, 2, 4, 5};

    auto count = std::ranges::count(numbers, 2);

    std::cout << count;

    // Output: 3
    Finding the maximum element in a range:
    std::vector<int> numbers{1, 2, 3, 4, 5};

    auto max_element = std::ranges::max_element(numbers);

    std::cout << *max_element;

    // Output: 5
    ```
1. Sorting a range in ascending order:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{5, 3, 1, 4, 2};

    std::ranges::sort(numbers);

    for (int n : numbers) {
        std::cout << n << ' ';
    }

    // Output: 1 2 3 4 5
    ```
1. Reversing the order of elements in a range:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};

    std::ranges::reverse(numbers);

    for (int n : numbers) {
        std::cout << n << ' ';
    }

    // Output: 5 4 3 2 1
    ```
1. Checking if any element in a range satisfies a condition:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};

    bool has_even = std::ranges::any_of(numbers, [](int n){ return n % 2 == 0; });

    std::cout << std::boolalpha << has_even;

    // Output: true
    ```
1. Taking the first 3 elements from a range:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};

    auto first_three = numbers | std::views::take(3);

    for (int n : first_three) {
        std::cout << n << ' ';
    }

    // Output: 1 2 3
    ```
1. Skipping the first 2 elements in a range:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};

    auto skip_two = numbers | std::views::drop(2);

    for (int n : skip_two) {
        std::cout << n << ' ';
    }

    // Output: 3 4 5
    ```
1. Checking if two ranges are equal:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers1{1, 2, 3};
    std::vector<int> numbers2{1, 2, 3};

    bool equal = std::ranges::equal(numbers1, numbers2);

    std::cout << std::boolalpha << equal;

    // Output: true
    ```
1. Applying a function to each element in a range and storing the results:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};
    std::vector<int> doubled_numbers;

    std::ranges::transform(numbers, std::back_inserter(doubled_numbers), [](int n){ return n * 2; });

    for (int n : doubled_numbers) {
        std::cout << n << ' ';
    }

    // Output: 2 4 6 8 10
    ```
1. Checking if a range is sorted in ascending order:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};

    bool is_sorted = std::ranges::is_sorted(numbers);

    std::cout << std::boolalpha << is_sorted;

    // Output: true
    ```
1. Finding the first occurrence of a value in a range:
    ```cpp {title="ranges.cpp"}
    std::vector<int> numbers{1, 2, 3, 4, 5};

    auto iter = std::ranges::find(numbers, 3);

    if (iter != numbers.end()) {
        std::cout << *iter;
    } else {
        std::cout << "Not found";
    }

    // Output: 3
    ```

## Conclusion

Ranges in C++20 provide a powerful and expressive way to work with sequences of elements. They simplify code by replacing manual loops and iterator manipulation with a higher-level, composable abstraction. Ranges offer improved code readability, reusability, and performance. They enable a more functional programming style in C++, making code more concise and expressive.

As you explore C++20 and beyond, consider incorporating ranges into your code to harness their benefits and streamline your development process.

I hope this article provides a clear understanding of what ranges are in C++20 and how they can simplify your code. Happy coding!

