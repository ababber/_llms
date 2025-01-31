# _llms

> Notes for my journey through llms.

## to do

* [ ] determine max hardware settings on local environment and quantize models accordingly
* [ ] review quantization methods to find optimal method for local environment
* [ ] use `gguf my repo` to quantize `deepseek r1` then `llama 3.3`
* [ ] if `hf` repo constraints prevent quantizing either `deepseek r1` or `llama 3.3`, containerize `gguf my repo` and quantize the remaining model locally
* [ ] create a wrapper or whitelist for files in `/usr/local/lib` w/ [whitelist unbrewed files](https://superuser.com/questions/656578/warning-unbrewed-dylibs-were-found-in-usr-local-lib)
* [X] research a solution to manage `huggingface` models locally
* [X] download compatible models from [`huggingface` models](https://huggingface.co/models)
* [X] `brew doctor`, determine how `Unbrewed dylibs, header files, and static libraries...` were generated

## `llama.cpp` build resources

* due to the older hardware (intel mac w/ amd egpu) constraints of my local machine, `llama.cpp` will be used to run models locally
* [llama.cpp git repo](https://github.com/ggerganov/llama.cpp)
* [vulkan backend](https://github.com/ggerganov/llama.cpp/blob/master/docs/build.md#vulkan)

## `env` setup

* create a `venv` for `llama.cpp` using `pyenv`
* ensure `huggingface-cli` is installed via `pip`, `brew`, etc...
* `huggingface-cli login`
* use [huggingface access token](https://huggingface.co/settings/tokens) to `login`
* ensure `git lfs` is installed and is in `PATH`
* `git lfs --version`
* `git lfs install`
* `clone llama.cpp` and `cmake` if not already done, refer to [llama.cpp build resources](#llamacpp-build-resources)
* `pip install -r requirements.txt` in `/llama.cpp` to convert `huggingface` model and quantize models
* create a `/dir` on the same level as `/llama.cpp` for models downloaded from `huggingface`
* clone models into new `/dir`

## models compatible with `llama.cpp` from [ggml-org](https://huggingface.co/ggml-org)

* manual download links:
  * [download: ggml-org/meta-llama-3.1-8b-instruct-q4_0.gguf](https://huggingface.co/ggml-org/Meta-Llama-3.1-8B-Instruct-Q4_0-GGUF/resolve/main/meta-llama-3.1-8b-instruct-q4_0.gguf?download=true)
  * [download: ggml-org/gemma-1.1-7b-it.Q4_K_M.gguf](https://huggingface.co/ggml-org/gemma-1.1-7b-it-Q4_K_M-GGUF/resolve/main/gemma-1.1-7b-it.Q4_K_M.gguf?download=true)
* `git clone` links:
  * [`git clone` ggml-org/meta-llama-3.1-8b-instruct-q4_0.gguf](https://huggingface.co/ggml-org/Meta-Llama-3.1-8B-Instruct-Q4_0-GGUF)
  * [`git clone` ggml-org/gemma-1.1-7b-it.Q4_K_M.gguf](https://huggingface.co/ggml-org/gemma-1.1-7b-it-Q4_K_M-GGUF)

## notes and refs on how to quantize

* [safetensors or hf](https://huggingface.co/docs/safetensors/index)
* if not in `.gguf`, [how to add model manually using py script](https://github.com/ggerganov/llama.cpp/blob/master/docs/development/HOWTO-add-model.md)
* [llama.cpp quantize ref](https://github.com/ggerganov/llama.cpp/tree/master/examples/quantize)
* simplest solution is to [gguf my repo](https://huggingface.co/spaces/ggml-org/gguf-my-repo)

## containerize `hf space`

* [run hf space as container](https://huggingface.co/docs/hub/spaces-run-with-docker)
* [rocm containers](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/how-to/docker.html)
* [rocm and docker](https://github.com/ROCm/ROCm-docker/blob/master/quick-start.md)
* [rocm docs](https://rocm.docs.amd.com/en/latest/)

## `llama.cpp` usage

* [llama.cpp example](https://github.com/ggerganov/llama.cpp/blob/master/examples/main/README.md)
* download models from `huggingface` to `~/llama.cpp/models`
* ensure models are in `.gguf` format
* benchmark:

```sh
cd ~/llama.cpp
# example from docs
./build/bin/llama-bench --flash-attn 1 --model "./models/<GET-MODELS-IN-GGUF-FROM-HUGGING-FACE>"

# assuming file structure from `env` setup
./build/bin/llama-bench --flash-attn 1 --model ../llm-models/Meta-Llama-3.1-8B-Instruct-Q4_0-GGUF/meta-llama-3.1-8b-instruct-q4_0.gguf

# open Activity Monitor to confirm llama-bench running on gpu
```

* simple test:

```sh
cd ~/llama.cpp
# start interactive mode
./build/bin/llama-cli -m ../llm-models/gemma-1.1-7b-it-Q4_K_M-GGUF/gemma-1.1-7b-it.Q4_K_M.gguf
```
