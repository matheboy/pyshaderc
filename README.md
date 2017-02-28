# PyShaderc

## What is it ?

This module allow you to compile glsl to Spir-V in Python.
It leverage power of the great [shaderc](https://github.com/google/shaderc)
library to compile glsl.


## API

```
def set_include_paths(paths):
    """Return include paths

    This function allow you to update the include paths.
    Include paths are used with #include <file>.

    Args:
        paths (list[str]): List of paths
    """

def compile_file_into_spirv(filepath, stage, optimization='size',
                            warnings_as_errors=False):
    """Compile shader file into Spir-V binary.

    This function uses shaderc to compile your glsl file code into Spir-V
    code.

    Args:
        filepath (strs): Absolute path to your shader file
        stage (str): Pipeline stage in ['vert', 'tesc', 'tese', 'geom',
                     'frag', 'comp']
        optimization (str): 'zero' (no optimization) or 'size' (reduce size)
        warnings_as_errors (bool): Turn warnings into errors

    Returns:
        bytes: Compiled Spir-V binary.

    Raises:
        CompilationError: If compilation fails.
    """

def compile_into_spirv(raw, stage, filepath, language="glsl",
                       optimization='size', suppress_warnings=False,
                       warnings_as_errors=False):
    """Compile shader code into Spir-V binary.

    This function uses shaderc to compile your glsl or hlsl code into Spir-V
    code. You can refer to the shaderc documentation.

    Args:
        raw (bytes): glsl or hlsl code (bytes format, not str)
        stage (str): Pipeline stage in ['vert', 'tesc', 'tese', 'geom',
                     'frag', 'comp']
        filepath (str): Absolute path of the file (needed for #include)
        language (str): 'glsl' or 'hlsl'
        optimization (str): 'zero' (no optimization) or 'size' (reduce size)
        suppress_warnings (bool): True to suppress warnings
        warnings_as_errors (bool): Turn warnings into errors

    Returns:
        bytes: Compiled Spir-V binary.

    Raises:
        CompilationError: If compilation fails.
    """
```

## How to use it

### Getting started

So simple !

```
import pyshaderc
spirv = pyshaderc.compile_file_into_spirv('/tmp/myshader.vs.glsl', 'vert')
```

If you want more control, you can use the lower-level function
`compile_into_spirv`.

### Include support

Pyshaderc supports `#include` preprocessor thanks to shaderc.
There are two ways to use it:

**`#include "myfile"`**

Include file relatively, it's intuitive.

**`#include <myfile>`**

Like in C, you can include from a list of path. To use this way to include,
you must call `set_include_paths` with a list of directory to search for
before compiling your glsl code.


## How to build

**`libshaderc_combined.a`**

PyShaderc is statically linked with `libshaderc_combined.a`. To build this
static library, follow the shaderc README.
Nevertheless, there is a small thing to note, when you compile shaderc,
you must add the -fpic field.
To do so, add `-DCMAKE_C_FLAGS=-fpic -DCMAKE_CXX_FLAGS=-fpic` when you call `cmake`.
