## Bug Description

Since NVIDIA driver version 461.09, doing D3D11/DXGI rendering inside the
Win10 window movement/resize modal loop introduces stuttering of the 
entire window movement/resizing action. See below for reproducing
the problem.

## Build

Needs a somewhat a recent Visual Studio compiler toolchain and cmake.

```
> mkdir build
> cd build
> cmake ..
> cmake --build .
```

The created exe is in a subdirectory: **Debug/demo.exe**

## Reproduction Steps

On a Win10 machine with an NVIDIA graphics card (in my case RTX2070):

1. install driver version **461.09**
2. run the demo executable
3. move and resize the window, note how the movement and resizing stutters
4. now install the previous driver version **460.89**
5. run the demo.exe again, note how window movement and resizing is smooth


