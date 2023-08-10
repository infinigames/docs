# Code walkthrough

The dimensium base library only contains some frequently-typed codes and world/dimensium/chunk/(block/entity/item) data structures.

if you type `tree` you will get such these results (2023/8/10 uncommited changes):

```
.
├── CMakeLists.txt
├── LICENSE
├── README.md
└── src
    ├── CMakeLists.txt
    ├── base
    │   ├── base.hpp
    │   ├── definitions.hpp
    │   ├── exceptions.cpp
    │   ├── exceptions.hpp
    │   └── freq_used_exceptions.hpp
    ├── precompile.hpp
    ├── utility
    │   ├── allocation
    │   │   └── allocator.hpp
    │   ├── log
    │   │   ├── log_message.hpp
    │   │   ├── log_stream.cpp
    │   │   └── log_stream.hpp
    │   └── uuid
    │       ├── uuid.cpp
    │       └── uuid.hpp
    └── world
        ├── block.hpp
        ├── dimension.hpp
        ├── item.hpp
        ├── namespace_id.hpp
        └── world.hpp
```

This doc will introduce these codes, although it's still just-started developing.

## Build system

**We use [CMake] as build system**. [CMake] is a cross-platform, cross-compiler MAkeFile-generation tool, but not only a generation tool, but also a comprehensive building[^1]/testing[^2]/packaging[^3] toolset.

This is the root buildsystem file. *(2023/8/10 uncommited changes)*

```CMake title="/CMakeLists.txt"
cmake_minimum_required(VERSION 3.26)
project(
    dimensium_base
    LANGUAGES
        C CXX
)

set(CMAKE_CXX_STANDARD 20) # (1)!
set(CMAKE_CXX_STANDARD_REQUIRED ON)
    


set(CMAKE_EXPORT_COMPILE_COMMANDS ON)


add_subdirectory(src)
```
{.annotate}

1.  We use C++20 but still keep the "header-source file" style, which means **we won't use modules in dimensium-base code**. But we will use some C++20 library and language feature, such `std::jthread` we used in `class LogStream` [^4], `concept`s we use everywhere in **future [^5]** *fully-template-based classes and generic programming libraries [^6]*.

This file is the root `CMakeLists.txt` file, and it doesn't contains any building processes, only some general settings.

This is the file contains building processes.

```CMake title="/src/CMakeLists.txt"
file(
    GLOB_RECURSE
        DIMENSIUM_BASE_SOURCES  # (1)!

    *.c *.cc *.C *.cpp *.cxx 
)

## configuration options

if(${BUILD_SHARED_LIBS})
    add_library(
        dimensium_base SHARED
        ${DIMENSIUM_BASE_SOURCES}
    )
else()
    add_library(
        dimensium_base
        ${DIMENSIUM_BASE_SOURCES}
    )
endif()


target_include_directories(
    dimensium_base PUBLIC
        ./
        ../lib/
)

# doesn't linked to a library now 
# target_link_libraries(
#     dimensium_base PUBLIC
          # libraries which will linked soon 
          # (2)!
# )
```

1.  This `file()` invocation will deleted when necessary. Because CMake doesn't know when to regenerate the build system while adding a source without changing the `CMakeLists.txt`. Here is the official suggestion written in the CMake documentation:

    **Note**

    We do not recommend using `GLOB` to collect a list of source files from your source tree. If no `CMakeLists.txt` file changes when a source is added or removed then the generated build system cannot know when to ask CMake to regenerate. The `CONFIGURE_DEPENDS` flag may not work reliably on all generators, or if a new generator is added in the future that cannot support it, projects using it will be stuck. Even if `CONFIGURE_DEPENDS` works reliably, there is still a cost to perform the check on every rebuild. 

2. We haven't linked [RocksDB] and other "linkable" libraries with dimensium-base, so these lines was commented.





[^1]: CMake. It's main page is [here](https://cmake.org).
[^2]: CTest. It's a tool which makes automatically testing (CI) possible. It's bundled with [CMake].
[^3]: CPack. It's another tool to generate installer (for Windows), packages (`deb`, `rpm`, etc.), and other deployments.
[^4]: `LogStream` is a class, which provides synchroized logging output using some simple member functions. It prints it's own logging message called `LogMessage` which defined in `src/utility/log/log_message.hpp` instead of normal `std::string`, in order to print some debug message, including `std::thread::id` and log levels defined in `LogMessage::Type` (a `enum class`).
[^5]: Unfortunately, it is **just available in *future***.
[^6]: We're planning to use the full power of `template` classes and `concept`s in our libraries, including [dimensium-base], *dimensium-core* (which is still in design) and more.

[CMake]: https://cmake.org
[dimensium-base]: https://github.com/dimensium-base
[RocksDB]: https://rocksdb.org
