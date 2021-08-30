---
layout: page
title: Google Summer of Code 2021 - Enhancing clang-bind
short: GSoC 2021 clang-bind
permalink: /gsoc-2021/clang-bind/
---

**Student:** [Divyanshu Madan][divyanshu]

**Mentors:** [Kunal Tyagi][kunal], [Lars Glud][lars]


## Enhancing clang-bind
Enhancing the [Point Cloud Library](https://github.com/PointCloudLibrary)’s [clang-bind](https://github.com/PointCloudLibrary/clang-bind) project to fix and extend its existing capabilities: the aim is to fix existing issues and increase the coverage of the existing scope to develop python bindings of [PCL](https://github.com/PointCloudLibrary/pcl).

* [Merged PRs](#merged-prs)
  * [Introduce clang_utils.py, compilation_database.py, and related changes](#introduce-clang_utils.py,-compilation_database.py-and-related-changes)
  * [Introduce and utilise treelib](#introduce-and-utilise-treelib)
  * [Restructure and remove unnecessary files](#restructure-and-remove-unnecessary-files)
  * [Develop CMake frontend](#develop-cmake-frontend)
  * [Changes in parse.py](#changes-in-parse-py)
* [In-review PRs](#in-review-prs)
  * [Binding flow](#binding-flow)
  * [Use docker](#use-docker)
* [Things missing](#things-missing)
  * [pybind code generation](#pybind-code-generation)
  * [Integration tests](#integration-tests)

###  1. <a name='merged-prs'></a>Merged PRs
####  1.1. <a name='introduce-clang_utils.py,-compilation_database.py-and-related-changes'></a>Introduce clang_utils.py, compilation_database.py, and related changes [#6]( https://github.com/PointCloudLibrary/clang-bind/pull/6)
- Introduce `clang_utils.py`
  - We use the [inspect](https://docs.python.org/3/library/inspect.html#module-inspect) module, which provides several useful functions to help get information about live objects such as modules, classes, methods, functions, etc.
  - The [getmembers](https://docs.python.org/3/library/inspect.html#inspect.getmembers) function retrieves the members of an object such as a class or module though it triggers execution instead of doing static analysis.
    - This leads to errors, particularly on properties of classes in cindex.py, which causes segmentation errors or raises an Exception if a particular condition is not satisfied.
    - To curb this, we fetch the members statically. We define a custom function `getmembers_static` based on the one in the inspect module.
  - We then use the function to create utility functions for `is_`, `get_`, and `@property` methods for the following libclang objects:
    - [CursorKind](https://github.com/llvm/llvm-project/blob/1acd9a1a29ac30044ecefb6613485d5d168f66ca/clang/bindings/python/clang/cindex.py#L657): describes the kind of entity that a cursor points to.
    - [Cursor](https://github.com/llvm/llvm-project/blob/1acd9a1a29ac30044ecefb6613485d5d168f66ca/clang/bindings/python/clang/cindex.py#L1415): represents a reference to an element within the AST. It acts as a kind of iterator.
    - [Type](https://github.com/llvm/llvm-project/blob/1acd9a1a29ac30044ecefb6613485d5d168f66ca/clang/bindings/python/clang/cindex.py#L2180): represents the type of an element in the abstract syntax tree.
- Introduce `compilation_database.py`: Loads `compile_commands.json` from a given directory. This is useful to get compilation arguments for a file from the [compilation database](https://clang.llvm.org/docs/JSONCompilationDatabase.html).
- Relevant changes in `parse.py`, `generate.py`, and unit tests.

####  1.2. <a name='introduce-and-utilise-treelib'></a>Introduce and utilise treelib [#20](https://github.com/PointCloudLibrary/clang-bind/pull/20)
- Earlier, we were creating the tree structure using python dictionaries, in which a node had the following structure:
  ```py
  parsed_node = {
          "depth": node.get("depth"),
          "line": cursor.location.line,
          "column": cursor.location.column,
          "tokens": [x.spelling for x in cursor.get_tokens()],
          "cursor_kind": {
              **cursor_kind_utils.get_check_functions_dict(),
              **cursor_kind_utils.get_get_functions_dict(),
              **cursor_kind_utils.get_properties_dict(),
          },
          "cursor": {
              **cursor_utils.get_check_functions_dict(),
              **cursor_utils.get_get_functions_dict(),
              **cursor_utils.get_properties_dict(),
          },
          "type": {
              **cursor_type_utils.get_check_functions_dict(),
              **cursor_type_utils.get_get_functions_dict(),
              **cursor_type_utils.get_properties_dict(),
          },
          "members": [],
  }
  ```
  While this worked fine, we improved this by using a tree-based python package, instead of handling it via dictionaries. It also helps us reduce code during tree traversal by using pre-defined methods.
- We used [treelib](https://treelib.readthedocs.io/en/latest/) in `parse.py` to generate the tree from the traversal of the file's AST. This included:
  - Creating a `Node` class (as a data holder class). This contains parsed information about a cursor.
  - Utilise treelib in `Parse` class to create a similar structure as before.
- In unit tests, we use the [path_to_leaves](https://treelib.readthedocs.io/en/latest/treelib.html#treelib.tree.Tree.paths_to_leaves) function to check the parsed tree's nodes: imitating the prior tests' structure. We also now use clang objects directly for assertions (earlier, we were using string comparisons).

####  1.3. <a name='restructure-and-remove-unnecessary-files'></a>Restructure and remove unnecessary files [#23](https://github.com/PointCloudLibrary/clang-bind/pull/23)
- Earlier, since this project was meant to have resided in the [pcl](https://github.com/PointCloudLibrary/pcl) repo and used only for binding PCL, the `clang-bind` directory structure was designed accordingly: the code was present in `bindings/python` directory.
- Now, that we aim to create a general-purpose tool which is not specific to `pcl`, we updated the project to have the desired directory structure, for easier imports and better segregation:
  ```
  clang-bind
  ├── clang_bind
  │   ├── bind.py
  │   ├── clang_utils.py
  │   ├── cmake_frontend.py
  │   ├── generate.py
  │   ├── interface.py
  │   ├── parse.py
  │   ├── templates
  │   │   └── NAMESPACE
  │   └── utils.py
  ├── CMakeLists.txt
  ├── CMakeLists.txt.in
  ├── conanfile.txt
  ├── discussion.md
  ├── docs
  │   ├── make.bat
  │   ├── Makefile
  │   └── source
  │       ├── conf.py
  │       ├── index.rst
  │       ├── _static
  │       └── _templates
  ├── pytest.ini
  ├── README.md
  ├── requirements.txt
  ├── setup.py
  ├── tests
  │   ├── test_generate.py
  │   ├── test_parse.py
  │   └── test_project
  │       ├── CMakeLists.txt
  │       ├── include
  │       │   └── clang_bind_test
  │       │       └── function.hpp
  │       └── src
  │           └── simple.cpp
  └── third_party
      └── conan.cmake
  ```
- We performed clean-up: removing unnecessary old files, using `PYTHONPATH` instead of `context.py`, etc.

####  1.4. <a name='develop-cmake-frontend'></a>Develop CMake frontend [#24](https://github.com/PointCloudLibrary/clang-bind/pull/24)
- [CMake File API](https://cmake.org/cmake/help/git-stage/manual/cmake-file-api.7.html): CMake provides a file-based API that clients may use to get semantic information about the buildsystems CMake generates. Clients may use the API by writing query files to a specific location in a build tree to request zero or more Object Kinds. When CMake generates the buildsystem in that build tree it will read the query files and write reply files for the client to read.
  - API v1 directory structure:
    ```
    .cmake
    └── api
        └── v1
            ├── query
            │   └── codemodel-v2
            └── reply
    ```
  - Step-by-step (via https://silizium.io/post/cmake_build_information/):
    1. Navigate to your cmake project top-level directory.
    2. Create a build directory `build/`.
    3. Create the API directory `build/.cmake/api/v1/query`.
    4. Create the file `build/.cmake/api/v1/query/codemodel-v2` and leave it empty.
    5. Re-run cmake (the configuration step).
    6. CMake should have automatically created the directory `build/.cmake/api/v1/reply` containing the replies to our query.
    
    [Have a look at the shell script](https://github.com/PointCloudLibrary/clang-bind/blob/7bdcbcdf3d7bbc9ad706a5184d634f4524d29428/init_bindings.sh#L19-L21).
  - We introduce `cmake_frontend.py`, in which we get the CMake build information from the `reply/` directory via JSON manipulation.
    - We parse the [Object Kind](https://cmake.org/cmake/help/git-stage/manual/cmake-file-api.7.html#object-kinds) codemodel via `reply/codemodel-v2-<unspecified>.json` to get a list of build system targets. Such targets are created by calls to `add_executable()`, `add_library()`, and `add_custom_target()`, excluding imported targets and interface libraries (which do not generate any build rules).
    - We then access the target's corresponding `reply/target-<name>-<unspecified>.json` to get various information like sources, dependencies, etc.
- We move the class `CompilationDatabase` to `cmake_frontend.py` and remove `compilation_database.py`.
- We introduce [Sphinx-compatible documentation (RTD format)](https://sphinx-rtd-tutorial.readthedocs.io/en/latest/docstrings.html#the-sphinx-docstring-format) to the project.

####  1.5. <a name='changes-in-parse-py'></a>Changes in parse.py [#27](https://github.com/PointCloudLibrary/clang-bind/pull/27)
- Change identifier type of the AST:
  - For a tree node, we were using `clang_bind.parse.Node` (data holder class) objects as identifiers (`treelib.Tree.identifier`). 
  - This created a problem while attempting to create subtrees from the tree:
    - Subtree creation (and other functions) uses copy functions (from `copy` module) internally, which lead to:
      `ValueError: ctypes objects containing pointers cannot be pickled`
    - This is caused because the data holder class had clang pointers (cursors, `clang.cindex.Cursor`) which uses `ctypes` internally. Thus, their copies couldn't be created and subsequently, any tree operations couldn't be performed.
  - This can be solved by a simple logic: instead of using the data holder objects (and thus `ctypes`) as identifiers, we use UUIDs as identifiers and map them to the data holder objects. 
    - This won't create any difference in the tree representation as the tree printing operations rely on the `tag`, not the identifier and we are keeping the tag same (`repr` of the data holder).
    - The UUID creation process is handled internally by `treelib`, so we just let it create UUIDs and we map them to the data holder in our `clang_bind.parse.Parse` class.
  - We also refactor `clang_bind.parse.Node` (data holder) class to `clang_bind.parse.ParsedInfo` as it created confusion while using `treelib.Node` objects in our code
  - Corresponding changes in `parse.py`:
    - The new tree uses uuids (internal to `treelib`) as node identifiers instead of the Node (now ParsedInfo) objects.
    - Refactor `Node` class to `ParsedInfo`.
    - Introduced `_parsed_info_map`.
    - Add corresponding new helper functions.
  - Corresponding changes in `test_parse.py`:
    - The new tree has UUIDs as identifiers (internal), as opposed to the `Node` (now `ParsedInfo`) objects.
- Improve structure of tests in `test_parse.py`:
  - Use a class, remove the hard-coded filename, remove the dependency of each function on `tmp_path`: use `tempfile` module instead, file writing method changes.


###  2. <a name='in-review-prs'></a>In-review PRs
####  2.1. <a name='binding-flow'></a>Binding flow [#28](https://github.com/PointCloudLibrary/clang-bind/pull/28)
- Introduce `bind.py`
  - Using the CMake frontend, generate a binding database of all the files for a list of targets:
    ```py
    self.binding_db[target] = {
        "source_dir": source_dir,  # source dir containing C++ targets
        "output_dir": output_dir,  # output dir
        "inclusion_sources": inclusion_sources,  # inclusions for the target
        "files": [  # list of files' information
            {
                "source": cpp_source,
                "compiler_arguments": CompilationDatabase(build_dir).get_compilation_arguments(cpp_source),
            }
            for cpp_source in cpp_sources
        ],
    }
    ```
  - We can then use this database for functions such as parsing each file (for each target):
    ```py
    for file in target.get("files"):
        parsed_tree = Parse(
            Path(source_dir, file.get("source")),
            inclusion_sources,
            file.get("compiler_arguments"),
        ).get_tree()

        file.update({"parsed_tree": parsed_tree})  # update db
    ```
- Changes in `parse.py`
  - `_is_valid_child` function checks if the child is valid:
    - Old condition:
      - The child should be in the same file as the parent.
    - New condition:
      - Either child should be in the same file as the parent or,
      - the child should be in the list of valid inclusion sources.
- **LOGIC**
  - Earlier model:
    - We were not sure as to how different source files will be dealt with i.e. how they will interact with each other:
    - Whether we will parse and bind each one separately, which doesn't make sense (ex. to bind definition-only `.h` files) or,
    - Handle inclusions gracefully:
      - The ideal structure of a C++ project for binding is the standard one:
        - Implementation in `.cpp` source files.
        - Definitions in `.h` sources files.
        - `.h` included in `.cpp`.
      - `pcl` isn't an ideal project as it contains implementation in `.hpp` files (ex. see [point_types.hpp](https://github.com/PointCloudLibrary/pcl/blob/master/common/include/pcl/impl/point_types.hpp))
      - Thus, we cannot implement the model of creating bindings just for `.cpp` files and including the file's `INCLUSION_DIRECTIVES` directly in generated pybind code ([See Open3D's bindings for reference](https://github.com/isl-org/Open3D/tree/master/cpp/open3d))
    - [Have a look at pcl's parsed output using the old model.](https://github.com/divmadan/pcl_resources/tree/main/0)
  - New model:
    - The solution that we found (that should work) is we let the cursor traverse to the target's (or even whole project's, customizable) `INCLUSION_DIRECTIVES` and get the code from there.
    - This allows us to only parse the `.cpp` source files by getting the valid `INCLUSION_DIRECTIVES` (because we don't want to include std or 3rd party inclusions' code in our parsed tree) through CMake frontend.
    - This significantly reduced the number of files we needed to handle during pybind code generation (only `.cpp` source files) while also enabling all the required context to be managed in a single file.
    - [Have a look at pcl's parsed output using the new model.](https://github.com/divmadan/pcl_resources/tree/main/1)

####  2.2. <a name='use-docker'></a>Use docker [#29](https://github.com/PointCloudLibrary/clang-bind/pull/29)
- Introduce an ubuntu-based Dockerfile and use it in the `pytest.yml` Github Actions.

###  3. <a name='things-missing'></a>Things missing
####  3.1. <a name='pybind-code-generation'></a>pybind code generation
- The generation logic is tricky. While we can take away some for-loop complexity by using templates, it still is a difficult thing to generate pybind code generically.
- The issues are context generation and entity state management: 
  - Given a file, we want to generate correct contexts for each entity i.e. structs, functions, etc. from the entity's cursor's children.
  - Ex. for a `STRUCT_DECL`: inheritance, namespace scope, members, functions etc. and in turn their children (recursively), all should be used to create a state for the `STRUCT_DECL`. This should then be plugged in the context of the tree root `TRANSLATION_UNIT` and updated to generate the pybind code for the file.
- Apart from that, we also need to maintain a database of namespace resolved C++ entities because we are now including all the parsed information in the `.cpp` source files. This leads to multiple instances of entities in a single file, and all of them need to be carefully examined and the database and the entity state updated accordingly.
- From a practical perspective, have a look at the parsed tree for `point_types.cpp`: [point_types.txt](https://github.com/divmadan/pcl_resources/blob/main/1/parsed_txt-pcl%40cd3b3f954/common/src/point_types.txt).
- This couldn't have been possible in the time frame: we need to develop a comprehensive plan to handle the generation logic.

####  3.2. <a name='integration-tests'></a>Integration tests
- To be followed after we sort out the generation logic. Due to my lack of expertise in `pcl`, this requires a detailed discussion regarding the test cases.

[divyanshu]: https://github.com/divmadan
[kunal]: https://github.com/kunaltyagi
[lars]: https://github.com/larshg
