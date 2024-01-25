# Stable Diffusion on [tinygrad](https://github.com/tinygrad/tinygrad) WebGPU

This is a WebGPU port of Stable Diffusion in [tinygrad](https://github.com/tinygrad/tinygrad).<br>
Try it out [here](https://softwiredtech.github.io/stable-diffusion-webgpu/)!<br>
The python code I wrote to compile and export the model can be found [here](https://github.com/tinygrad/tinygrad/blob/master/examples/webgpu/stable_diffusion/compile.py)

## How it works

The Stable Diffusion model is exported in three parts:
- textModel
- diffusor
- decoder

If you open [net.js](./net.js) you can see all the WebGPU kernels that are involved in the inference.<br>
When you open the page for the first time, the model will be downloaded from [huggingface](https://huggingface.co/wpmed/tinygrad-sd-f16/resolve/main) in tinygrad's safetensor format. The model is in f16 to optimize download speed.<br>
Since the computation is done in `f32` in the model, and since `shader-f16` is not yet supported in production Chrome, the model is decompressed to `f32` using [f16-to-f32-gpu](https://github.com/wpmed92/f16-to-f32-gpu) WebGPU-based `f16` decompression library.<br>
When you open the site a second time, the model will be loaded from the `IndexedDB` cache into which it was saved on first visit. If you have a full cache hit, the model will be decompressed, compiled and ready to use. If you have a cache miss (usually due to `QuotaExceededError`), the model will be redownloaded.

## License

MIT
