## Bug Description

Since NVIDIA driver version 461.09, doing D3D11/DXGI rendering inside the
Win10 window movement/resize modal loop introduces stuttering of the 
entire window movement/resizing action. See below for reproducing
the problem.

## Clone and Build

Needs a somewhat recent Visual Studio compiler toolchain and cmake.

```
> git clone https://github.com/floooh/nvidia-driver-issue
> cd nvidia-driver-issue
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

## More Info

The problematic code is here, this creates a WM_TIMER on WM_MOVESIZEENTER/EXIT
to perform rendering while moving or resizing the window:

https://github.com/floooh/nvidia-driver-issue/blob/53032347fd5ba1979455879b65d8a4d331d74b63/src/sokol_app.h#L5987-L6011

Out-commenting the present-call here (which calls IDXGISwapChain::Present()) removes the
stuttering, so the stuttering seems to be related DXGI's framebuffer presentation,
not to the actual D3D11 rendering(?):

https://github.com/floooh/nvidia-driver-issue/blob/53032347fd5ba1979455879b65d8a4d331d74b63/src/sokol_app.h#L5996
