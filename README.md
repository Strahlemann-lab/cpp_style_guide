# C++ Style Guide

## 1. General Guidelines

- `main()` should only control program flow; move logic to separate classes/functions
- Separate interface (header) and implementation (source) files
- Functions should be small, clear, and testable
- Prefer modern C++ features: auto, range-based for, enum class, std::array, std::unique\_ptr,\
  std::jthread, etc.
- Do not use `using namespace std;` or other global using directives

- Comments: use Doxygen-style or `//` comments to describe intentions
- Use TODO comments for temporary or imperfect solutions (e.g., `// TODO: task`)

## 2. Formatting

- Non-ASCII characters should be rare; always use UTF-8 encoding

- Limit line length to **120 characters**
- Use only **spaces**, with **4-space indentation**
- Maintain consistent spacing: after commas, around binary operators, and after control keywords\
  Examples: `a, b, c`, `if (x == y) {}`, `value = a + b`, `for (auto o : objects) {}`
- Always place opening braces on a new line (next line brace style)\
  Applies to: if, for, while, functions, classes, structs, namespaces, etc.
- Always add an end-of-namespace comment:

```cpp
namespace definitions
{
    // code

} // !definitions
```

- Order class members by access level: private → protected → public
- For structs, reverse order: public → protected → private\
  This increases visibility and avoids accidental exposure:

```cpp
class Rectangle
{
    int a, b, c;

protected:
    void Load();
    bool HasSize();

public:
    void Render();
};
```

## 3. Header Structure & Include Guards

- Header files must use `.hpp` extension
- **Separate interface/declaration (**`**) from implementation (**`**)**
- No implementation in header files (except templates)
- Do not use `using` directives in headers
- All headers must use include guards to avoid multiple inclusion
- Format for guard macro: `<PROJECT>_<PATH>_<FILE>_H_`

Header structure should follow this order:

1. Include guard
2. Include ordering:
   1. Own headers
   2. C system headers (e.g., `<cstdint>`)
   3. C++ standard library headers
   4. Other libraries
3. Declaration

Example:

```cpp
#ifndef MY_PROJECT_HTTP_SERVER_H_
#define MY_PROJECT_HTTP_SERVER_H_

#include "my_project/base/logger.h"

#include <cstdint>
#include <string>
#include <vector>

namespace my_project
{
    class HttpServer
    {
        // exampleCode
    };

} // !my_project

#endif // MY_PROJECT_HTTP_SERVER_H_
```

## 4. Header Comments for main.cpp

Each main source file (e.g., `main.cpp`) should begin with a comment block:

Example:

```cpp
//
// @name       : project_main
// @description: Main entry point for project
// @author     : name
// @version    : 1.0.0
// @date       : 0000-12-31
// @git-hub    : github.com/user/project
// @license    : MIT
//
```

## 5. Naming Conventions

| Identifier Type  | Example           | Convention                    |
| ---------------- | ----------------- | ----------------------------- |
| Macros           | MAX\_BUFFER\_SIZE | UPPER\_CASE\_WITH\_UNDERSCORE |
| Constants        | MAX\_SPEED\_LIMIT | UPPER\_CASE\_WITH\_UNDERSCORE |
| Variables        | current\_speed    | snake\_case                   |
| Parameters       | file\_path        | snake\_case                   |
| Functions        | CalculateArea     | PascalCase                    |
| Classes          | HttpServer        | PascalCase                    |
| Structs          | AudioConfig       | PascalCase                    |
| Enums            | ErrorCode         | PascalCase                    |
| Enum Values      | ENUM\_VALUE\_ONE  | UPPER\_CASE\_WITH\_UNDERSCORE |
| Member Variables | current\_speed\_  | snake\_case\_                 |
| Namespaces       | my\_project       | snake\_case                   |
| Templates        | T                 | Capital single letter         |
| Type Aliases     | MyCallback        | PascalCase                    |
| Files            | https\_server.hpp | snake\_case                   |

Notes:

- Match file names to class names (e.g., `http_server.hpp`, `http_server.cpp`)
- Avoid abbreviations or unclear names
- Names must be descriptive and consistent
- Use PascalCase for classes, functions, and type names
- Use snake\_case for everything else

## 6. Type Usage

**General:**

- Use **classic types (int, char, bool, etc.)** by default
- Use **fixed-width types (int32\_t, uint64\_t, etc.) only when width is critical**
- Always `#include <cstdint>` when using fixed-width types
- Use `size_t` and `ptrdiff_t` for sizes and pointer arithmetic

**Type Overview:**

| Type                           | Value Range (signed / unsigned)                      | Description                          |
| ------------------------------ | ---------------------------------------------------- | ------------------------------------ |
| char / unsigned char           | -128 to 127 / 0 to 255                               | Basic character type (usually 8 bit) |
| short / unsigned short         | -32,768 to 32,767 / 0 to 65,535                      | At least 16-bit integers             |
| int / unsigned int             | -2,147,483,648 to 2,147,483,647 / 0 to 4,294,967,295 | At least 16 or 32 bit integers       |
| long / unsigned long           | Platform dependent (≥32 bit)                         | At least 32-bit integers             |
| long long / unsigned long long | ±9.2×10¹⁸ / 0 to \~1.8×10¹⁹                          | At least 64-bit integers             |
| float                          | ±1.2×10⁻³⁸ to ±3.4×10³⁸                              | 32-bit IEEE 754 floating point       |
| double                         | ±2.2×10⁻³⁰⁸ to ±1.8×10³⁰⁸                            | 64-bit IEEE 754 floating point       |
| long double                    | Implementation-defined (≥ double)                    | Extended precision float             |

| Type                               | Value Range (signed / unsigned)                      | Description                               | Include     |
| ---------------------------------- | ---------------------------------------------------- | ----------------------------------------- | ----------- |
| int8\_t / uint8\_t                 | -128 to 127 / 0 to 255                               | Exactly 8-bit integers                    | `<cstdint>` |
| int16\_t / uint16\_t               | -32,768 to 32,767 / 0 to 65,535                      | Exactly 16-bit integers                   | -           |
| int32\_t / uint32\_t               | -2,147,483,648 to 2,147,483,647 / 0 to 4,294,967,295 | Exactly 32-bit integers                   | -           |
| int64\_t / uint64\_t               | ±9.2×10¹⁸ / 0 to \~1.8×10¹⁹                          | Exactly 64-bit integers                   | -           |
| int\_least8\_t / uint\_least8\_t   | ≥ -128 to ≥ 127 / ≥ 0 to ≥ 255                       | Smallest type with ≥8 bits                | -           |
| int\_least16\_t / uint\_least16\_t | ≥ -32,768 to ≥ 32,767 / ≥ 0 to ≥ 65,535              | Smallest type with ≥16 bits               | -           |
| int\_least32\_t / uint\_least32\_t | ≥ -2.1×10⁹ to ≥ 2.1×10⁹ / ≥ 0 to ≥ 4.2×10⁹           | Smallest type with ≥32 bits               | -           |
| int\_least64\_t / uint\_least64\_t | ≥ ±9×10¹⁸ / ≥ 0 to \~1.8×10¹⁹                        | Smallest type with ≥64 bits               | -           |
| int\_fast8\_t / uint\_fast8\_t     | Platform dependent, ≥8 bits                          | Fastest type with ≥8 bits                 | -           |
| int\_fast16\_t / uint\_fast16\_t   | Platform dependent, ≥16 bits                         | Fastest type with ≥16 bits                | -           |
| int\_fast32\_t / uint\_fast32\_t   | Platform dependent, ≥32 bits                         | Fastest type with ≥32 bits                | -           |
| int\_fast64\_t / uint\_fast64\_t   | Platform dependent, ≥64 bits                         | Fastest type with ≥64 bits                | -           |
| intptr\_t / uintptr\_t             | Platform dependent (e.g. 32/64-bit)                  | Integer type capable of storing a pointer | -           |
| ptrdiff\_t / size\_t               | -2ⁿ⁻¹ to 2ⁿ⁻¹-1 / 0 to 2ⁿ-1 (architecture dependent) | pointer subtraction / object/array sizes  | `<cstddef>` |


## 7. Error Handling

- Avoid exceptions (`try/catch`) unless absolutely necessary (e.g., third-party libraries)
- Handle errors via return values or status objects (e.g., `Result<T>`, `optional<T>`, `bool + enum`)

Example:

```cpp
struct Result
{
    bool success;
    std::string error_message;
};

Result ReadFile(const std::string& path);

if (!result.success)
{
    logger_.LogError(result.error_message);
}

// Or:

bool ReadFile(const std::string& path);

if (!ReadFile(path))
{
    logger_.LogError("Read failed");
}
```

- Use `assert()` only for debug checks, not for runtime error handling
- Always manage resources via RAII (e.g., smart pointers)

