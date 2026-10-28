# Integrating imnodes by using CMake

To let CMake manage imnodes as a dependency, add a new FetchContent block to the file CMakeLists.txt in the project root between the FetchContent block for stbi and the WIN32-only area:

```
FetchContent_Declare(
    imnodes
    GIT_REPOSITORY https://github.com/Nelarius/imnodes
    GIT_TAG v0.5
)
```

We will use the Git tag v0.5 here to avoid problems introduced by new commits to the master branch, resulting in not finding the ImGui source code.

Next, we make the imnodes code available and let CMake populate the variable for the source folder:

```
FetchContent_MakeAvailable(imnodes)
FetchContent_GetProperties(imnodes)
if(NOT imnodes_POPULATED)
    FetchContent_Populate(imnodes)
    add_subdirectory(${imnodes_SOURCE_DIR} EXCLUDE_FROM_ALL)
endif()
```

Then, we append the source directory variable for imnodes to the list of already existing external source files and include directories:

```
file(GLOB SOURCES
    ...
    ${imnodes_SOURCE_DIR}/imnodes.cpp
)
...
target_include_directories(... ${imnodes_SOURCE_DIR})
```

As a last step, we add a compiler definition to the existing add_definitions line:

```
add_definitions(... -DIMGUI_DEFINE_MATH_OPERATORS)
```