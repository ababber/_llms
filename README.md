# _llms

> Notes for my journey through llms.

## to do

* [ ] determine max settings on local environment
* [ ] download compatible models from [hugging face models](https://huggingface.co/models)
* [ ] research a solution to manage hugging face models locally
* [X] `brew doctor`, determine how `Unbrewed dylibs, header files, and static libraries...` were generated, and cleanup

## `llama.cpp` notes

* due to the older hardware (intel mac w/ amd egpu) constraints of my local machine, `llama.cpp` will be used to run models locally
* [llama.cpp git repo](https://github.com/ggerganov/llama.cpp)
* [vulkan backend](https://github.com/ggerganov/llama.cpp/blob/master/docs/build.md#vulkan)

## `llama.cpp` usage

* download models from `huggingface` to `~/llama.cpp/models`
* [`huggingface` gemma-1.1-7b-it.Q4_K_M.gguf](https://huggingface.co/ggml-org/gemma-1.1-7b-it-Q4_K_M-GGUF/resolve/main/gemma-1.1-7b-it.Q4_K_M.gguf?download=true)
* ensure models are in `.gguf` format
* benchmark:

```sh
cd ~/llama.cpp
./build/bin/llama-bench --flash-attn 1 --model "./models/<GET-MODELS-IN-GGUF-FROM-HUGGING-FACE>"

# example from docs
./build/bin/llama-bench --flash-attn 1 --model "./models/gemma-1.1-7b-it.Q4_K_M.gguf"

# open Activity Monitor to confirm llama-bench running on gpu
```

* simple test:

```sh
cd ~/llama.cpp
./build/bin/llama-cli -m ./models/gemma-1.1-7b-it.Q4_K_M.gguf -p "Once upon a time"
```
