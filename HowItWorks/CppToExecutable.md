## C++ text code to machine executable code 00110011 (Mac OS-X example)
------


### Let create c++ file in any editor with source code

```
// 1.1 Add directives for preprocessor
#define MY_CONSTANT1 512
#define MY_CONSTANT2 532
#define getmin(a,b) a < b ? a : b

// 1.2 We can include all function declarations from <stdio.h> or just write printf declaration and link library <stdio.h> during linking step
// #include <stdio.h> // Skip header it just add printf same function declaration
extern "C" {
  int printf(const char* format, ...);
}

// 2. Main function with code
int main(int argc, const char* argv[])
{
  // 3. Inside main funciton can call helper functions
  int min = getmin(MY_CONSTANT1, MY_CONSTANT2);
  printf("The min number is: %i\n", min);
  return 0;
}
```

printf function [documentation](http://www.cplusplus.com/reference/cstdio/printf/)

------


### Compilation STEP 1. C++ preprocessor step
`g++ -Wall -std=c++11 -arch x86_64 -mmacosx-version-min=10.12 -E -P -o AfterPreprocessingStepSource.cpp source.cpp`

If you want to see full preprocessor output with line number information remove `-P` option

Information about [preprocessor output](https://gcc.gnu.org/onlinedocs/gcc-4.3.6/cpp/Preprocessor-Output.html)

------

### Compilation STEP 2. C++ compilation to assembly code

`g++ -Wall -std=c++11 -arch x86_64 -mmacosx-version-min=10.12 -S  -o AfterCompileStepSource.s AfterPreprocessingStepSource.cpp`

OS-X [assembly language](https://developer.apple.com/library/prerelease/content/documentation/DeveloperTools/Reference/Assembler/000-Introduction/introduction.html#//apple_ref/doc/uid/TP30000851-CH211-SW1)

Helpful article about OS-X [assembly language syntax](http://www.idryman.org/blog/2014/12/02/writing-64-bit-assembly-on-mac-os-x/)

------

### Compilation STEP 3. Assembly compilation to object binary code
`as -arch x86_64 -o AfterAssemblyStepObjectFile.o AfterCompileStepSource.s`

We can see binary output with hexdump `hexdump -C AfterAssemblyStepObjectFile.o`

------

### Compilation STEP 4. Linking to exacutable file
ld -arch x86_64 -macosx_version_min 10.12 -dynamic -lSystem -o program AfterAssemblyStepObjectFile.o

Important options:
  - Dynamic linking with OS-X system libraries `-dynamic -lSystem`
  - Prevent issues with desync versions after assembly compilation `-arch x86_64 -macosx_version_min 10.12`

We can see executable output with hexdump `hexdump -C program`

------

### All in one step with full g++ compilation
`g++ -Wall -std=c++11 -arch x86_64 -mmacosx-version-min=10.12 -o program source.cpp`