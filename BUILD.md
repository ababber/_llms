# build `llama.cpp` on `macOS`

## notes on building and using `llama.cpp` w/ `vulkan` backend

* using `vulkan sdk` creates unbrewed files in `/usr/local/lib`
* using `brew install vulkan-headers glslang molten-vk shaderc vulkan-tools` causes issues with `vulkan env`

### [llama.cpp docs vulkan build](https://github.com/ggerganov/llama.cpp/blob/master/docs/build.md#vulkan)

* [install `vulkan sdk`](https://vulkan.lunarg.com/sdk/home#windows)
* `brew install cmake libomp`
* `vulkan sdk` causes `Unbrewed dylibs, header files, and static libraries...` to be generated b/c files are written directly to `/usr/local/lib`
* no issues while using llm model
* command_0:

```sh
# `brew libomp` and link to where `brew` installs it to fix `Could NOT find OpenMP_C OpenMP_CXX OpenMP`
cmake -B build -DGGML_METAL=OFF -DGGML_VULKAN=ON \
-DOpenMP_C_FLAGS=-fopenmp=lomp \
-DOpenMP_CXX_FLAGS=-fopenmp=lomp \
-DOpenMP_C_LIB_NAMES="libomp" \
-DOpenMP_CXX_LIB_NAMES="libomp" \
-DOpenMP_libomp_LIBRARY="$(brew --prefix)/opt/libomp/lib/libomp.dylib" \
-DOpenMP_CXX_FLAGS="-Xpreprocessor -fopenmp $(brew --prefix)/opt/libomp/lib/libomp.dylib -I$(brew --prefix)/opt/libomp/include" \
-DOpenMP_CXX_LIB_NAMES="libomp" \
-DOpenMP_C_FLAGS="-Xpreprocessor -fopenmp $(brew --prefix)/opt/libomp/lib/libomp.dylib -I$(brew --prefix)/opt/libomp/include"
```

* output_0:

```sh
-- The C compiler identification is AppleClang 16.0.0.16000026
-- The CXX compiler identification is AppleClang 16.0.0.16000026
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Library/Developer/CommandLineTools/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Library/Developer/CommandLineTools/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found Git: /usr/bin/git (found version "2.39.5 (Apple Git-154)")
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- ccache found, compilation results will be cached. Disable with GGML_CCACHE=OFF.
-- CMAKE_SYSTEM_PROCESSOR: x86_64
-- Including CPU backend
-- Accelerate framework found
-- Found OpenMP_C: -Xpreprocessor -fopenmp /usr/local/opt/libomp/lib/libomp.dylib -I/usr/local/opt/libomp/include (found version "5.1")
-- Found OpenMP_CXX: -Xpreprocessor -fopenmp /usr/local/opt/libomp/lib/libomp.dylib -I/usr/local/opt/libomp/include (found version "5.1")
-- Found OpenMP: TRUE (found version "5.1")
-- x86 detected
-- Adding CPU backend variant ggml-cpu: -march=native 
-- Looking for dgemm_
-- Looking for dgemm_ - found
-- Found BLAS: /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/System/Library/Frameworks/Accelerate.framework
-- BLAS found, Libraries: /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/System/Library/Frameworks/Accelerate.framework
-- BLAS found, Includes: 
-- Including BLAS backend
-- Found Vulkan: /usr/local/lib/libvulkan.dylib (found version "1.4.304") found components: glslc glslangValidator
-- Vulkan found
-- GL_KHR_cooperative_matrix supported by glslc
-- GL_NV_cooperative_matrix2 supported by glslc
-- Including Vulkan backend
-- Configuring done (2.6s)
-- Generating done (1.1s)
-- Build files have been written to: /Users/ankit/Playground/llama.cpp/build
```

* command_1:

```sh
cmake --build build --config Release
```

* `warnings` can be ignored b/c model is able to run locally
* output_1:

```sh
[  0%] Building C object ggml/src/CMakeFiles/ggml-base.dir/ggml.c.o
[  1%] Building C object ggml/src/CMakeFiles/ggml-base.dir/ggml-alloc.c.o
[  1%] Building CXX object ggml/src/CMakeFiles/ggml-base.dir/ggml-backend.cpp.o
[  2%] Building CXX object ggml/src/CMakeFiles/ggml-base.dir/ggml-opt.cpp.o
[  2%] Building CXX object ggml/src/CMakeFiles/ggml-base.dir/ggml-threading.cpp.o
[  3%] Building C object ggml/src/CMakeFiles/ggml-base.dir/ggml-quants.c.o
[  3%] Building CXX object ggml/src/CMakeFiles/ggml-base.dir/gguf.cpp.o
[  4%] Linking CXX shared library ../../bin/libggml-base.dylib
[  4%] Built target ggml-base
[  5%] Building CXX object ggml/src/ggml-vulkan/vulkan-shaders/CMakeFiles/vulkan-shaders-gen.dir/vulkan-shaders-gen.cpp.o
[  5%] Linking CXX executable ../../../../bin/vulkan-shaders-gen
[  5%] Built target vulkan-shaders-gen
[  6%] Generate vulkan shaders
ggml_vulkan: Generating and compiling shaders to SPIR-V
[  6%] Building CXX object ggml/src/ggml-vulkan/CMakeFiles/ggml-vulkan.dir/ggml-vulkan.cpp.o
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:1378:2: warning: extra ';' outside of a function is incompatible with C++98 [-Wc++98-compat-extra-semi]
 1378 | };
      |  ^
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:7010:16: warning: 'return' will never be executed [-Wunreachable-code-return]
 7010 |         return false;
      |                ^~~~~
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:8166:15: warning: 'break' will never be executed [-Wunreachable-code-break]
 8166 |             } break;
      |               ^~~~~
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:8125:15: warning: 'break' will never be executed [-Wunreachable-code-break]
 8125 |             } break;
      |               ^~~~~
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:8048:15: warning: 'break' will never be executed [-Wunreachable-code-break]
 8048 |             } break;
      |               ^~~~~
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:7997:13: warning: 'break' will never be executed [-Wunreachable-code-break]
 7997 |             break;
      |             ^~~~~
6 warnings generated.
[  7%] Building CXX object ggml/src/ggml-vulkan/CMakeFiles/ggml-vulkan.dir/ggml-vulkan-shaders.cpp.o
[  7%] Linking CXX shared library ../../../bin/libggml-vulkan.dylib
[  7%] Built target ggml-vulkan
[  7%] Building C object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu.c.o
cc: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
In file included from /Users/ankit/Playground/llama.cpp/ggml/src/ggml-cpu/ggml-cpu.c:40:
/usr/local/opt/libomp/include/omp.h:54:9: warning: ISO C restricts enumerator values to range of 'int' (2147483648 is too large) [-Wpedantic]
   54 |         omp_sched_monotonic = 0x80000000
      |         ^                     ~~~~~~~~~~
/usr/local/opt/libomp/include/omp.h:411:7: warning: ISO C restricts enumerator values to range of 'int' (18446744073709551615 is too large) [-Wpedantic]
  411 |       KMP_ALLOCATOR_MAX_HANDLE = UINTPTR_MAX
      |       ^                          ~~~~~~~~~~~
/usr/local/opt/libomp/include/omp.h:427:7: warning: ISO C restricts enumerator values to range of 'int' (18446744073709551615 is too large) [-Wpedantic]
  427 |       KMP_MEMSPACE_MAX_HANDLE = UINTPTR_MAX
      |       ^                         ~~~~~~~~~~~
/usr/local/opt/libomp/include/omp.h:471:39: warning: ISO C restricts enumerator values to range of 'int' (18446744073709551615 is too large) [-Wpedantic]
  471 |     typedef enum omp_event_handle_t { KMP_EVENT_MAX_HANDLE = UINTPTR_MAX } omp_event_handle_t;
      |                                       ^                      ~~~~~~~~~~~
4 warnings generated.
[  8%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[  8%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu-aarch64.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[  8%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu-hbm.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[  9%] Building C object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu-quants.c.o
cc: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[  9%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu-traits.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[ 10%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/amx/amx.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[ 10%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/amx/mmq.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[ 11%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/llamafile/sgemm.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[ 11%] Linking CXX shared library ../../bin/libggml-cpu.dylib
[ 11%] Built target ggml-cpu
[ 11%] Building CXX object ggml/src/ggml-blas/CMakeFiles/ggml-blas.dir/ggml-blas.cpp.o
[ 12%] Linking CXX shared library ../../../bin/libggml-blas.dylib
[ 12%] Built target ggml-blas
[ 12%] Building CXX object ggml/src/CMakeFiles/ggml.dir/ggml-backend-reg.cpp.o
[ 13%] Linking CXX shared library ../../bin/libggml.dylib
[ 13%] Built target ggml
[ 14%] Building CXX object src/CMakeFiles/llama.dir/llama.cpp.o
[ 14%] Building CXX object src/CMakeFiles/llama.dir/llama-adapter.cpp.o
[ 15%] Building CXX object src/CMakeFiles/llama.dir/llama-arch.cpp.o
[ 15%] Building CXX object src/CMakeFiles/llama.dir/llama-batch.cpp.o
[ 16%] Building CXX object src/CMakeFiles/llama.dir/llama-chat.cpp.o
[ 16%] Building CXX object src/CMakeFiles/llama.dir/llama-context.cpp.o
[ 17%] Building CXX object src/CMakeFiles/llama.dir/llama-grammar.cpp.o
[ 17%] Building CXX object src/CMakeFiles/llama.dir/llama-hparams.cpp.o
[ 18%] Building CXX object src/CMakeFiles/llama.dir/llama-impl.cpp.o
[ 18%] Building CXX object src/CMakeFiles/llama.dir/llama-kv-cache.cpp.o
[ 19%] Building CXX object src/CMakeFiles/llama.dir/llama-mmap.cpp.o
[ 19%] Building CXX object src/CMakeFiles/llama.dir/llama-model-loader.cpp.o
[ 20%] Building CXX object src/CMakeFiles/llama.dir/llama-model.cpp.o
[ 20%] Building CXX object src/CMakeFiles/llama.dir/llama-quant.cpp.o
[ 21%] Building CXX object src/CMakeFiles/llama.dir/llama-sampling.cpp.o
[ 21%] Building CXX object src/CMakeFiles/llama.dir/llama-vocab.cpp.o
[ 21%] Building CXX object src/CMakeFiles/llama.dir/unicode.cpp.o
[ 22%] Building CXX object src/CMakeFiles/llama.dir/unicode-data.cpp.o
[ 22%] Linking CXX shared library ../bin/libllama.dylib
[ 22%] Built target llama
[ 22%] Building CXX object common/CMakeFiles/build_info.dir/build-info.cpp.o
[ 22%] Built target build_info
[ 23%] Building CXX object common/CMakeFiles/common.dir/arg.cpp.o
[ 23%] Building CXX object common/CMakeFiles/common.dir/common.cpp.o
[ 24%] Building CXX object common/CMakeFiles/common.dir/console.cpp.o
[ 24%] Building CXX object common/CMakeFiles/common.dir/json-schema-to-grammar.cpp.o
[ 25%] Building CXX object common/CMakeFiles/common.dir/log.cpp.o
[ 25%] Building CXX object common/CMakeFiles/common.dir/ngram-cache.cpp.o
[ 26%] Building CXX object common/CMakeFiles/common.dir/sampling.cpp.o
[ 26%] Building CXX object common/CMakeFiles/common.dir/speculative.cpp.o
[ 27%] Linking CXX static library libcommon.a
[ 27%] Built target common
[ 28%] Building CXX object tests/CMakeFiles/test-tokenizer-0.dir/test-tokenizer-0.cpp.o
[ 28%] Linking CXX executable ../bin/test-tokenizer-0
[ 28%] Built target test-tokenizer-0
[ 28%] Building CXX object tests/CMakeFiles/test-sampling.dir/test-sampling.cpp.o
[ 29%] Building CXX object tests/CMakeFiles/test-sampling.dir/get-model.cpp.o
[ 29%] Linking CXX executable ../bin/test-sampling
[ 29%] Built target test-sampling
[ 30%] Building CXX object tests/CMakeFiles/test-grammar-parser.dir/test-grammar-parser.cpp.o
[ 30%] Building CXX object tests/CMakeFiles/test-grammar-parser.dir/get-model.cpp.o
[ 31%] Linking CXX executable ../bin/test-grammar-parser
[ 31%] Built target test-grammar-parser
[ 31%] Building CXX object tests/CMakeFiles/test-grammar-integration.dir/test-grammar-integration.cpp.o
[ 32%] Building CXX object tests/CMakeFiles/test-grammar-integration.dir/get-model.cpp.o
[ 32%] Linking CXX executable ../bin/test-grammar-integration
[ 32%] Built target test-grammar-integration
[ 33%] Building CXX object tests/CMakeFiles/test-llama-grammar.dir/test-llama-grammar.cpp.o
[ 33%] Building CXX object tests/CMakeFiles/test-llama-grammar.dir/get-model.cpp.o
[ 34%] Linking CXX executable ../bin/test-llama-grammar
[ 34%] Built target test-llama-grammar
[ 34%] Building CXX object tests/CMakeFiles/test-json-schema-to-grammar.dir/test-json-schema-to-grammar.cpp.o
[ 35%] Building CXX object tests/CMakeFiles/test-json-schema-to-grammar.dir/get-model.cpp.o
[ 35%] Linking CXX executable ../bin/test-json-schema-to-grammar
[ 35%] Built target test-json-schema-to-grammar
[ 36%] Building CXX object tests/CMakeFiles/test-tokenizer-1-bpe.dir/test-tokenizer-1-bpe.cpp.o
[ 36%] Linking CXX executable ../bin/test-tokenizer-1-bpe
[ 36%] Built target test-tokenizer-1-bpe
[ 37%] Building CXX object tests/CMakeFiles/test-tokenizer-1-spm.dir/test-tokenizer-1-spm.cpp.o
[ 37%] Linking CXX executable ../bin/test-tokenizer-1-spm
[ 37%] Built target test-tokenizer-1-spm
[ 37%] Building CXX object tests/CMakeFiles/test-log.dir/test-log.cpp.o
[ 37%] Building CXX object tests/CMakeFiles/test-log.dir/get-model.cpp.o
[ 38%] Linking CXX executable ../bin/test-log
[ 38%] Built target test-log
[ 39%] Building CXX object tests/CMakeFiles/test-arg-parser.dir/test-arg-parser.cpp.o
[ 39%] Building CXX object tests/CMakeFiles/test-arg-parser.dir/get-model.cpp.o
[ 40%] Linking CXX executable ../bin/test-arg-parser
[ 40%] Built target test-arg-parser
[ 40%] Building CXX object tests/CMakeFiles/test-chat-template.dir/test-chat-template.cpp.o
[ 41%] Building CXX object tests/CMakeFiles/test-chat-template.dir/get-model.cpp.o
[ 41%] Linking CXX executable ../bin/test-chat-template
[ 41%] Built target test-chat-template
[ 42%] Building CXX object tests/CMakeFiles/test-gguf.dir/test-gguf.cpp.o
[ 42%] Building CXX object tests/CMakeFiles/test-gguf.dir/get-model.cpp.o
[ 43%] Linking CXX executable ../bin/test-gguf
[ 43%] Built target test-gguf
[ 44%] Building CXX object tests/CMakeFiles/test-backend-ops.dir/test-backend-ops.cpp.o
[ 44%] Building CXX object tests/CMakeFiles/test-backend-ops.dir/get-model.cpp.o
[ 44%] Linking CXX executable ../bin/test-backend-ops
[ 44%] Built target test-backend-ops
[ 44%] Building CXX object tests/CMakeFiles/test-model-load-cancel.dir/test-model-load-cancel.cpp.o
[ 45%] Building CXX object tests/CMakeFiles/test-model-load-cancel.dir/get-model.cpp.o
[ 45%] Linking CXX executable ../bin/test-model-load-cancel
[ 45%] Built target test-model-load-cancel
[ 45%] Building CXX object tests/CMakeFiles/test-autorelease.dir/test-autorelease.cpp.o
[ 46%] Building CXX object tests/CMakeFiles/test-autorelease.dir/get-model.cpp.o
[ 46%] Linking CXX executable ../bin/test-autorelease
[ 46%] Built target test-autorelease
[ 47%] Building CXX object tests/CMakeFiles/test-barrier.dir/test-barrier.cpp.o
[ 47%] Building CXX object tests/CMakeFiles/test-barrier.dir/get-model.cpp.o
[ 48%] Linking CXX executable ../bin/test-barrier
[ 48%] Built target test-barrier
[ 49%] Building CXX object tests/CMakeFiles/test-quantize-fns.dir/test-quantize-fns.cpp.o
[ 49%] Building CXX object tests/CMakeFiles/test-quantize-fns.dir/get-model.cpp.o
[ 50%] Linking CXX executable ../bin/test-quantize-fns
[ 50%] Built target test-quantize-fns
[ 50%] Building CXX object tests/CMakeFiles/test-quantize-perf.dir/test-quantize-perf.cpp.o
[ 51%] Building CXX object tests/CMakeFiles/test-quantize-perf.dir/get-model.cpp.o
[ 51%] Linking CXX executable ../bin/test-quantize-perf
[ 51%] Built target test-quantize-perf
[ 52%] Building CXX object tests/CMakeFiles/test-rope.dir/test-rope.cpp.o
[ 52%] Building CXX object tests/CMakeFiles/test-rope.dir/get-model.cpp.o
[ 53%] Linking CXX executable ../bin/test-rope
[ 53%] Built target test-rope
[ 53%] Building C object tests/CMakeFiles/test-c.dir/test-c.c.o
[ 54%] Linking C executable ../bin/test-c
[ 54%] Built target test-c
[ 55%] Building CXX object examples/batched-bench/CMakeFiles/llama-batched-bench.dir/batched-bench.cpp.o
[ 55%] Linking CXX executable ../../bin/llama-batched-bench
[ 55%] Built target llama-batched-bench
[ 56%] Building CXX object examples/batched/CMakeFiles/llama-batched.dir/batched.cpp.o
[ 56%] Linking CXX executable ../../bin/llama-batched
[ 56%] Built target llama-batched
[ 57%] Building CXX object examples/embedding/CMakeFiles/llama-embedding.dir/embedding.cpp.o
[ 57%] Linking CXX executable ../../bin/llama-embedding
[ 57%] Built target llama-embedding
[ 58%] Building CXX object examples/eval-callback/CMakeFiles/llama-eval-callback.dir/eval-callback.cpp.o
[ 58%] Linking CXX executable ../../bin/llama-eval-callback
[ 58%] Built target llama-eval-callback
[ 59%] Building CXX object examples/gbnf-validator/CMakeFiles/llama-gbnf-validator.dir/gbnf-validator.cpp.o
[ 59%] Linking CXX executable ../../bin/llama-gbnf-validator
[ 59%] Built target llama-gbnf-validator
[ 59%] Building C object examples/gguf-hash/CMakeFiles/sha256.dir/deps/sha256/sha256.c.o
[ 59%] Built target sha256
[ 60%] Building C object examples/gguf-hash/CMakeFiles/xxhash.dir/deps/xxhash/xxhash.c.o
[ 60%] Built target xxhash
[ 61%] Building C object examples/gguf-hash/CMakeFiles/sha1.dir/deps/sha1/sha1.c.o
[ 61%] Built target sha1
[ 61%] Building CXX object examples/gguf-hash/CMakeFiles/llama-gguf-hash.dir/gguf-hash.cpp.o
[ 62%] Linking CXX executable ../../bin/llama-gguf-hash
[ 62%] Built target llama-gguf-hash
[ 62%] Building CXX object examples/gguf-split/CMakeFiles/llama-gguf-split.dir/gguf-split.cpp.o
[ 63%] Linking CXX executable ../../bin/llama-gguf-split
[ 63%] Built target llama-gguf-split
[ 63%] Building CXX object examples/gguf/CMakeFiles/llama-gguf.dir/gguf.cpp.o
[ 64%] Linking CXX executable ../../bin/llama-gguf
[ 64%] Built target llama-gguf
[ 64%] Building CXX object examples/gritlm/CMakeFiles/llama-gritlm.dir/gritlm.cpp.o
[ 65%] Linking CXX executable ../../bin/llama-gritlm
[ 65%] Built target llama-gritlm
[ 65%] Building CXX object examples/imatrix/CMakeFiles/llama-imatrix.dir/imatrix.cpp.o
[ 66%] Linking CXX executable ../../bin/llama-imatrix
[ 66%] Built target llama-imatrix
[ 66%] Building CXX object examples/infill/CMakeFiles/llama-infill.dir/infill.cpp.o
[ 67%] Linking CXX executable ../../bin/llama-infill
[ 67%] Built target llama-infill
[ 68%] Building CXX object examples/llama-bench/CMakeFiles/llama-bench.dir/llama-bench.cpp.o
[ 68%] Linking CXX executable ../../bin/llama-bench
[ 68%] Built target llama-bench
[ 68%] Building CXX object examples/lookahead/CMakeFiles/llama-lookahead.dir/lookahead.cpp.o
[ 69%] Linking CXX executable ../../bin/llama-lookahead
[ 69%] Built target llama-lookahead
[ 69%] Building CXX object examples/lookup/CMakeFiles/llama-lookup.dir/lookup.cpp.o
[ 70%] Linking CXX executable ../../bin/llama-lookup
[ 70%] Built target llama-lookup
[ 70%] Building CXX object examples/lookup/CMakeFiles/llama-lookup-create.dir/lookup-create.cpp.o
[ 71%] Linking CXX executable ../../bin/llama-lookup-create
[ 71%] Built target llama-lookup-create
[ 71%] Building CXX object examples/lookup/CMakeFiles/llama-lookup-merge.dir/lookup-merge.cpp.o
[ 72%] Linking CXX executable ../../bin/llama-lookup-merge
[ 72%] Built target llama-lookup-merge
[ 72%] Building CXX object examples/lookup/CMakeFiles/llama-lookup-stats.dir/lookup-stats.cpp.o
[ 73%] Linking CXX executable ../../bin/llama-lookup-stats
[ 73%] Built target llama-lookup-stats
[ 74%] Building CXX object examples/main/CMakeFiles/llama-cli.dir/main.cpp.o
[ 74%] Linking CXX executable ../../bin/llama-cli
[ 74%] Built target llama-cli
[ 74%] Building CXX object examples/parallel/CMakeFiles/llama-parallel.dir/parallel.cpp.o
[ 74%] Linking CXX executable ../../bin/llama-parallel
[ 74%] Built target llama-parallel
[ 75%] Building CXX object examples/passkey/CMakeFiles/llama-passkey.dir/passkey.cpp.o
[ 75%] Linking CXX executable ../../bin/llama-passkey
[ 75%] Built target llama-passkey
[ 76%] Building CXX object examples/perplexity/CMakeFiles/llama-perplexity.dir/perplexity.cpp.o
[ 76%] Linking CXX executable ../../bin/llama-perplexity
[ 76%] Built target llama-perplexity
[ 77%] Building CXX object examples/quantize/CMakeFiles/llama-quantize.dir/quantize.cpp.o
[ 77%] Linking CXX executable ../../bin/llama-quantize
[ 77%] Built target llama-quantize
[ 78%] Building CXX object examples/retrieval/CMakeFiles/llama-retrieval.dir/retrieval.cpp.o
[ 78%] Linking CXX executable ../../bin/llama-retrieval
[ 78%] Built target llama-retrieval
[ 79%] Generating loading.html.hpp
[ 79%] Generating index.html.gz.hpp
[ 79%] Building CXX object examples/server/CMakeFiles/llama-server.dir/server.cpp.o
[ 80%] Linking CXX executable ../../bin/llama-server
[ 80%] Built target llama-server
[ 80%] Building CXX object examples/save-load-state/CMakeFiles/llama-save-load-state.dir/save-load-state.cpp.o
[ 81%] Linking CXX executable ../../bin/llama-save-load-state
[ 81%] Built target llama-save-load-state
[ 82%] Building CXX object examples/run/CMakeFiles/llama-run.dir/run.cpp.o
[ 82%] Building CXX object examples/run/CMakeFiles/llama-run.dir/linenoise.cpp/linenoise.cpp.o
[ 83%] Linking CXX executable ../../bin/llama-run
[ 83%] Built target llama-run
[ 83%] Building CXX object examples/simple/CMakeFiles/llama-simple.dir/simple.cpp.o
[ 83%] Linking CXX executable ../../bin/llama-simple
[ 83%] Built target llama-simple
[ 84%] Building CXX object examples/simple-chat/CMakeFiles/llama-simple-chat.dir/simple-chat.cpp.o
[ 84%] Linking CXX executable ../../bin/llama-simple-chat
[ 84%] Built target llama-simple-chat
[ 85%] Building CXX object examples/speculative/CMakeFiles/llama-speculative.dir/speculative.cpp.o
[ 85%] Linking CXX executable ../../bin/llama-speculative
[ 85%] Built target llama-speculative
[ 86%] Building CXX object examples/speculative-simple/CMakeFiles/llama-speculative-simple.dir/speculative-simple.cpp.o
[ 86%] Linking CXX executable ../../bin/llama-speculative-simple
[ 86%] Built target llama-speculative-simple
[ 87%] Building CXX object examples/tokenize/CMakeFiles/llama-tokenize.dir/tokenize.cpp.o
[ 87%] Linking CXX executable ../../bin/llama-tokenize
[ 87%] Built target llama-tokenize
[ 88%] Building CXX object examples/tts/CMakeFiles/llama-tts.dir/tts.cpp.o
[ 88%] Linking CXX executable ../../bin/llama-tts
[ 88%] Built target llama-tts
[ 89%] Building CXX object examples/gen-docs/CMakeFiles/llama-gen-docs.dir/gen-docs.cpp.o
[ 89%] Linking CXX executable ../../bin/llama-gen-docs
[ 89%] Built target llama-gen-docs
[ 90%] Building CXX object examples/convert-llama2c-to-ggml/CMakeFiles/llama-convert-llama2c-to-ggml.dir/convert-llama2c-to-ggml.cpp.o
[ 90%] Linking CXX executable ../../bin/llama-convert-llama2c-to-ggml
[ 90%] Built target llama-convert-llama2c-to-ggml
[ 91%] Building CXX object examples/cvector-generator/CMakeFiles/llama-cvector-generator.dir/cvector-generator.cpp.o
[ 91%] Linking CXX executable ../../bin/llama-cvector-generator
[ 91%] Built target llama-cvector-generator
[ 92%] Building CXX object examples/export-lora/CMakeFiles/llama-export-lora.dir/export-lora.cpp.o
[ 92%] Linking CXX executable ../../bin/llama-export-lora
[ 92%] Built target llama-export-lora
[ 93%] Building CXX object examples/quantize-stats/CMakeFiles/llama-quantize-stats.dir/quantize-stats.cpp.o
[ 93%] Linking CXX executable ../../bin/llama-quantize-stats
[ 93%] Built target llama-quantize-stats
[ 94%] Building CXX object examples/llava/CMakeFiles/llava.dir/llava.cpp.o
[ 94%] Building CXX object examples/llava/CMakeFiles/llava.dir/clip.cpp.o
[ 94%] Built target llava
[ 94%] Linking CXX static library libllava_static.a
[ 94%] Built target llava_static
[ 95%] Linking CXX shared library ../../bin/libllava_shared.dylib
[ 95%] Built target llava_shared
[ 95%] Building CXX object examples/llava/CMakeFiles/llama-llava-cli.dir/llava-cli.cpp.o
[ 96%] Linking CXX executable ../../bin/llama-llava-cli
[ 96%] Built target llama-llava-cli
[ 96%] Building CXX object examples/llava/CMakeFiles/llama-minicpmv-cli.dir/minicpmv-cli.cpp.o
[ 97%] Linking CXX executable ../../bin/llama-minicpmv-cli
[ 97%] Built target llama-minicpmv-cli
[ 98%] Building CXX object examples/llava/CMakeFiles/llama-qwen2vl-cli.dir/qwen2vl-cli.cpp.o
[ 98%] Linking CXX executable ../../bin/llama-qwen2vl-cli
[ 98%] Built target llama-qwen2vl-cli
[ 99%] Building CXX object pocs/vdot/CMakeFiles/llama-vdot.dir/vdot.cpp.o
[ 99%] Linking CXX executable ../../bin/llama-vdot
[ 99%] Built target llama-vdot
[100%] Building CXX object pocs/vdot/CMakeFiles/llama-q8dot.dir/q8dot.cpp.o
[100%] Linking CXX executable ../../bin/llama-q8dot
[100%] Built target llama-q8dot
```

### [fix for `Could NOT find OpenMP_C OpenMP_CXX OpenMP`](https://stackoverflow.com/questions/60126203/how-do-i-get-cmake-to-find-openmp-c-openmp-cxx-etc)

* `brew install cmake git libomp vulkan-headers glslang molten-vk shaderc vulkan-tools`
* `cmake` command w/ explicit links to `env` vars for `libomp`, `vulkan-headers`, and `molten-vk`
* issue with `vulkan env` variable when not installed via sdk: `ggml_vulkan: WARNING: Instance extension VK_KHR_portability_enumeration not found` occurs during usage
* command_0:

```sh
cmake -B build -DLLAMA_CURL=ON -DGGML_METAL=OFF -DGGML_VULKAN=ON \
-DOpenMP_C_FLAGS=-fopenmp=lomp \
-DOpenMP_CXX_FLAGS=-fopenmp=lomp \
-DOpenMP_C_LIB_NAMES="libomp" \
-DOpenMP_CXX_LIB_NAMES="libomp" \
-DOpenMP_libomp_LIBRARY="$(brew --prefix)/opt/libomp/lib/libomp.dylib" \
-DOpenMP_CXX_FLAGS="-Xpreprocessor -fopenmp $(brew --prefix)/opt/libomp/lib/libomp.dylib -I$(brew --prefix)/opt/libomp/include" \
-DOpenMP_CXX_LIB_NAMES="libomp" \
-DOpenMP_C_FLAGS="-Xpreprocessor -fopenmp $(brew --prefix)/opt/libomp/lib/libomp.dylib -I$(brew --prefix)/opt/libomp/include" \
-DVulkan_INCLUDE_DIR="$(brew --prefix)/opt/vulkan-headers/include" \
-DVulkan_LIBRARY="$(brew --prefix)/opt/molten-vk/lib/libMoltenVK.dylib" \
-DVulkan_GLSLC_EXECUTABLE="$(brew --prefix)/opt/shaderc/bin/glslc" \
-DVulkan_GLSLANG_VALIDATOR_EXECUTABLE="$(brew --prefix)/opt/glslang/bin/glslangValidator"
```

* output_0:

```sh
-- The C compiler identification is AppleClang 16.0.0.16000026
-- The CXX compiler identification is AppleClang 16.0.0.16000026
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Library/Developer/CommandLineTools/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Library/Developer/CommandLineTools/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found Git: /usr/local/bin/git (found version "2.48.1")
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- ccache found, compilation results will be cached. Disable with GGML_CCACHE=OFF.
-- CMAKE_SYSTEM_PROCESSOR: x86_64
-- Including CPU backend
-- Accelerate framework found
-- Found OpenMP_C: -Xpreprocessor -fopenmp /usr/local/opt/libomp/lib/libomp.dylib -I/usr/local/opt/libomp/include (found version "5.1")
-- Found OpenMP_CXX: -Xpreprocessor -fopenmp /usr/local/opt/libomp/lib/libomp.dylib -I/usr/local/opt/libomp/include (found version "5.1")
-- Found OpenMP: TRUE (found version "5.1")
-- x86 detected
-- Adding CPU backend variant ggml-cpu: -march=native 
-- Looking for dgemm_
-- Looking for dgemm_ - found
-- Found BLAS: /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/System/Library/Frameworks/Accelerate.framework
-- BLAS found, Libraries: /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/System/Library/Frameworks/Accelerate.framework
-- BLAS found, Includes: 
-- Including BLAS backend
-- Found Vulkan: /usr/local/opt/molten-vk/lib/libMoltenVK.dylib (found version "1.4.306") found components: glslc glslangValidator
-- Vulkan found
-- GL_KHR_cooperative_matrix supported by glslc
-- GL_NV_cooperative_matrix2 supported by glslc
-- Including Vulkan backend
-- Found CURL: /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/usr/lib/libcurl.tbd (found version "8.7.1")
-- Configuring done (4.8s)
-- Generating done (1.1s)
-- Build files have been written to: /Users/ankit/Playground/llama.cpp/build
```

* command_1:

```sh
cmake --build build --config Release
```

* `warnings` can be ignored b/c model is able to run locally
* output_1:

```sh
[  0%] Building C object ggml/src/CMakeFiles/ggml-base.dir/ggml.c.o
[  1%] Building C object ggml/src/CMakeFiles/ggml-base.dir/ggml-alloc.c.o
[  1%] Building CXX object ggml/src/CMakeFiles/ggml-base.dir/ggml-backend.cpp.o
[  2%] Building CXX object ggml/src/CMakeFiles/ggml-base.dir/ggml-opt.cpp.o
[  2%] Building CXX object ggml/src/CMakeFiles/ggml-base.dir/ggml-threading.cpp.o
[  3%] Building C object ggml/src/CMakeFiles/ggml-base.dir/ggml-quants.c.o
[  3%] Building CXX object ggml/src/CMakeFiles/ggml-base.dir/gguf.cpp.o
[  4%] Linking CXX shared library ../../bin/libggml-base.dylib
[  4%] Built target ggml-base
[  5%] Building CXX object ggml/src/ggml-vulkan/vulkan-shaders/CMakeFiles/vulkan-shaders-gen.dir/vulkan-shaders-gen.cpp.o
[  5%] Linking CXX executable ../../../../bin/vulkan-shaders-gen
[  5%] Built target vulkan-shaders-gen
[  6%] Generate vulkan shaders
ggml_vulkan: Generating and compiling shaders to SPIR-V
[  6%] Building CXX object ggml/src/ggml-vulkan/CMakeFiles/ggml-vulkan.dir/ggml-vulkan.cpp.o
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:1378:2: warning: extra ';' outside of a function is incompatible with C++98 [-Wc++98-compat-extra-semi]
 1378 | };
      |  ^
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:7010:16: warning: 'return' will never be executed [-Wunreachable-code-return]
 7010 |         return false;
      |                ^~~~~
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:8166:15: warning: 'break' will never be executed [-Wunreachable-code-break]
 8166 |             } break;
      |               ^~~~~
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:8125:15: warning: 'break' will never be executed [-Wunreachable-code-break]
 8125 |             } break;
      |               ^~~~~
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:8048:15: warning: 'break' will never be executed [-Wunreachable-code-break]
 8048 |             } break;
      |               ^~~~~
/Users/ankit/Playground/llama.cpp/ggml/src/ggml-vulkan/ggml-vulkan.cpp:7997:13: warning: 'break' will never be executed [-Wunreachable-code-break]
 7997 |             break;
      |             ^~~~~
6 warnings generated.
[  7%] Building CXX object ggml/src/ggml-vulkan/CMakeFiles/ggml-vulkan.dir/ggml-vulkan-shaders.cpp.o
[  7%] Linking CXX shared library ../../../bin/libggml-vulkan.dylib
[  7%] Built target ggml-vulkan
[  7%] Building C object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu.c.o
cc: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
In file included from /Users/ankit/Playground/llama.cpp/ggml/src/ggml-cpu/ggml-cpu.c:40:
/usr/local/opt/libomp/include/omp.h:54:9: warning: ISO C restricts enumerator values to range of 'int' (2147483648 is too large) [-Wpedantic]
   54 |         omp_sched_monotonic = 0x80000000
      |         ^                     ~~~~~~~~~~
/usr/local/opt/libomp/include/omp.h:411:7: warning: ISO C restricts enumerator values to range of 'int' (18446744073709551615 is too large) [-Wpedantic]
  411 |       KMP_ALLOCATOR_MAX_HANDLE = UINTPTR_MAX
      |       ^                          ~~~~~~~~~~~
/usr/local/opt/libomp/include/omp.h:427:7: warning: ISO C restricts enumerator values to range of 'int' (18446744073709551615 is too large) [-Wpedantic]
  427 |       KMP_MEMSPACE_MAX_HANDLE = UINTPTR_MAX
      |       ^                         ~~~~~~~~~~~
/usr/local/opt/libomp/include/omp.h:471:39: warning: ISO C restricts enumerator values to range of 'int' (18446744073709551615 is too large) [-Wpedantic]
  471 |     typedef enum omp_event_handle_t { KMP_EVENT_MAX_HANDLE = UINTPTR_MAX } omp_event_handle_t;
      |                                       ^                      ~~~~~~~~~~~
4 warnings generated.
[  8%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[  8%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu-aarch64.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[  8%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu-hbm.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[  9%] Building C object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu-quants.c.o
cc: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[  9%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/ggml-cpu-traits.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[ 10%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/amx/amx.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[ 10%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/amx/mmq.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[ 11%] Building CXX object ggml/src/CMakeFiles/ggml-cpu.dir/ggml-cpu/llamafile/sgemm.cpp.o
c++: warning: /usr/local/opt/libomp/lib/libomp.dylib: 'linker' input unused [-Wunused-command-line-argument]
[ 11%] Linking CXX shared library ../../bin/libggml-cpu.dylib
[ 11%] Built target ggml-cpu
[ 11%] Building CXX object ggml/src/ggml-blas/CMakeFiles/ggml-blas.dir/ggml-blas.cpp.o
[ 12%] Linking CXX shared library ../../../bin/libggml-blas.dylib
[ 12%] Built target ggml-blas
[ 12%] Building CXX object ggml/src/CMakeFiles/ggml.dir/ggml-backend-reg.cpp.o
[ 13%] Linking CXX shared library ../../bin/libggml.dylib
[ 13%] Built target ggml
[ 14%] Building CXX object src/CMakeFiles/llama.dir/llama.cpp.o
[ 14%] Building CXX object src/CMakeFiles/llama.dir/llama-adapter.cpp.o
[ 15%] Building CXX object src/CMakeFiles/llama.dir/llama-arch.cpp.o
[ 15%] Building CXX object src/CMakeFiles/llama.dir/llama-batch.cpp.o
[ 16%] Building CXX object src/CMakeFiles/llama.dir/llama-chat.cpp.o
[ 16%] Building CXX object src/CMakeFiles/llama.dir/llama-context.cpp.o
[ 17%] Building CXX object src/CMakeFiles/llama.dir/llama-grammar.cpp.o
[ 17%] Building CXX object src/CMakeFiles/llama.dir/llama-hparams.cpp.o
[ 18%] Building CXX object src/CMakeFiles/llama.dir/llama-impl.cpp.o
[ 18%] Building CXX object src/CMakeFiles/llama.dir/llama-kv-cache.cpp.o
[ 19%] Building CXX object src/CMakeFiles/llama.dir/llama-mmap.cpp.o
[ 19%] Building CXX object src/CMakeFiles/llama.dir/llama-model-loader.cpp.o
[ 20%] Building CXX object src/CMakeFiles/llama.dir/llama-model.cpp.o
[ 20%] Building CXX object src/CMakeFiles/llama.dir/llama-quant.cpp.o
[ 21%] Building CXX object src/CMakeFiles/llama.dir/llama-sampling.cpp.o
[ 21%] Building CXX object src/CMakeFiles/llama.dir/llama-vocab.cpp.o
[ 21%] Building CXX object src/CMakeFiles/llama.dir/unicode.cpp.o
[ 22%] Building CXX object src/CMakeFiles/llama.dir/unicode-data.cpp.o
[ 22%] Linking CXX shared library ../bin/libllama.dylib
[ 22%] Built target llama
[ 22%] Building CXX object common/CMakeFiles/build_info.dir/build-info.cpp.o
[ 22%] Built target build_info
[ 23%] Building CXX object common/CMakeFiles/common.dir/arg.cpp.o
[ 23%] Building CXX object common/CMakeFiles/common.dir/common.cpp.o
[ 24%] Building CXX object common/CMakeFiles/common.dir/console.cpp.o
[ 24%] Building CXX object common/CMakeFiles/common.dir/json-schema-to-grammar.cpp.o
[ 25%] Building CXX object common/CMakeFiles/common.dir/log.cpp.o
[ 25%] Building CXX object common/CMakeFiles/common.dir/ngram-cache.cpp.o
[ 26%] Building CXX object common/CMakeFiles/common.dir/sampling.cpp.o
[ 26%] Building CXX object common/CMakeFiles/common.dir/speculative.cpp.o
[ 27%] Linking CXX static library libcommon.a
[ 27%] Built target common
[ 28%] Building CXX object tests/CMakeFiles/test-tokenizer-0.dir/test-tokenizer-0.cpp.o
[ 28%] Linking CXX executable ../bin/test-tokenizer-0
[ 28%] Built target test-tokenizer-0
[ 28%] Building CXX object tests/CMakeFiles/test-sampling.dir/test-sampling.cpp.o
[ 29%] Building CXX object tests/CMakeFiles/test-sampling.dir/get-model.cpp.o
[ 29%] Linking CXX executable ../bin/test-sampling
[ 29%] Built target test-sampling
[ 30%] Building CXX object tests/CMakeFiles/test-grammar-parser.dir/test-grammar-parser.cpp.o
[ 30%] Building CXX object tests/CMakeFiles/test-grammar-parser.dir/get-model.cpp.o
[ 31%] Linking CXX executable ../bin/test-grammar-parser
[ 31%] Built target test-grammar-parser
[ 31%] Building CXX object tests/CMakeFiles/test-grammar-integration.dir/test-grammar-integration.cpp.o
[ 32%] Building CXX object tests/CMakeFiles/test-grammar-integration.dir/get-model.cpp.o
[ 32%] Linking CXX executable ../bin/test-grammar-integration
[ 32%] Built target test-grammar-integration
[ 33%] Building CXX object tests/CMakeFiles/test-llama-grammar.dir/test-llama-grammar.cpp.o
[ 33%] Building CXX object tests/CMakeFiles/test-llama-grammar.dir/get-model.cpp.o
[ 34%] Linking CXX executable ../bin/test-llama-grammar
[ 34%] Built target test-llama-grammar
[ 34%] Building CXX object tests/CMakeFiles/test-json-schema-to-grammar.dir/test-json-schema-to-grammar.cpp.o
[ 35%] Building CXX object tests/CMakeFiles/test-json-schema-to-grammar.dir/get-model.cpp.o
[ 35%] Linking CXX executable ../bin/test-json-schema-to-grammar
[ 35%] Built target test-json-schema-to-grammar
[ 36%] Building CXX object tests/CMakeFiles/test-tokenizer-1-bpe.dir/test-tokenizer-1-bpe.cpp.o
[ 36%] Linking CXX executable ../bin/test-tokenizer-1-bpe
[ 36%] Built target test-tokenizer-1-bpe
[ 37%] Building CXX object tests/CMakeFiles/test-tokenizer-1-spm.dir/test-tokenizer-1-spm.cpp.o
[ 37%] Linking CXX executable ../bin/test-tokenizer-1-spm
[ 37%] Built target test-tokenizer-1-spm
[ 37%] Building CXX object tests/CMakeFiles/test-log.dir/test-log.cpp.o
[ 37%] Building CXX object tests/CMakeFiles/test-log.dir/get-model.cpp.o
[ 38%] Linking CXX executable ../bin/test-log
[ 38%] Built target test-log
[ 39%] Building CXX object tests/CMakeFiles/test-arg-parser.dir/test-arg-parser.cpp.o
[ 39%] Building CXX object tests/CMakeFiles/test-arg-parser.dir/get-model.cpp.o
[ 40%] Linking CXX executable ../bin/test-arg-parser
[ 40%] Built target test-arg-parser
[ 40%] Building CXX object tests/CMakeFiles/test-chat-template.dir/test-chat-template.cpp.o
[ 41%] Building CXX object tests/CMakeFiles/test-chat-template.dir/get-model.cpp.o
[ 41%] Linking CXX executable ../bin/test-chat-template
[ 41%] Built target test-chat-template
[ 42%] Building CXX object tests/CMakeFiles/test-gguf.dir/test-gguf.cpp.o
[ 42%] Building CXX object tests/CMakeFiles/test-gguf.dir/get-model.cpp.o
[ 43%] Linking CXX executable ../bin/test-gguf
[ 43%] Built target test-gguf
[ 44%] Building CXX object tests/CMakeFiles/test-backend-ops.dir/test-backend-ops.cpp.o
[ 44%] Building CXX object tests/CMakeFiles/test-backend-ops.dir/get-model.cpp.o
[ 44%] Linking CXX executable ../bin/test-backend-ops
[ 44%] Built target test-backend-ops
[ 44%] Building CXX object tests/CMakeFiles/test-model-load-cancel.dir/test-model-load-cancel.cpp.o
[ 45%] Building CXX object tests/CMakeFiles/test-model-load-cancel.dir/get-model.cpp.o
[ 45%] Linking CXX executable ../bin/test-model-load-cancel
[ 45%] Built target test-model-load-cancel
[ 45%] Building CXX object tests/CMakeFiles/test-autorelease.dir/test-autorelease.cpp.o
[ 46%] Building CXX object tests/CMakeFiles/test-autorelease.dir/get-model.cpp.o
[ 46%] Linking CXX executable ../bin/test-autorelease
[ 46%] Built target test-autorelease
[ 47%] Building CXX object tests/CMakeFiles/test-barrier.dir/test-barrier.cpp.o
[ 47%] Building CXX object tests/CMakeFiles/test-barrier.dir/get-model.cpp.o
[ 48%] Linking CXX executable ../bin/test-barrier
[ 48%] Built target test-barrier
[ 49%] Building CXX object tests/CMakeFiles/test-quantize-fns.dir/test-quantize-fns.cpp.o
[ 49%] Building CXX object tests/CMakeFiles/test-quantize-fns.dir/get-model.cpp.o
[ 50%] Linking CXX executable ../bin/test-quantize-fns
[ 50%] Built target test-quantize-fns
[ 50%] Building CXX object tests/CMakeFiles/test-quantize-perf.dir/test-quantize-perf.cpp.o
[ 51%] Building CXX object tests/CMakeFiles/test-quantize-perf.dir/get-model.cpp.o
[ 51%] Linking CXX executable ../bin/test-quantize-perf
[ 51%] Built target test-quantize-perf
[ 52%] Building CXX object tests/CMakeFiles/test-rope.dir/test-rope.cpp.o
[ 52%] Building CXX object tests/CMakeFiles/test-rope.dir/get-model.cpp.o
[ 53%] Linking CXX executable ../bin/test-rope
[ 53%] Built target test-rope
[ 53%] Building C object tests/CMakeFiles/test-c.dir/test-c.c.o
[ 54%] Linking C executable ../bin/test-c
[ 54%] Built target test-c
[ 55%] Building CXX object examples/batched-bench/CMakeFiles/llama-batched-bench.dir/batched-bench.cpp.o
[ 55%] Linking CXX executable ../../bin/llama-batched-bench
[ 55%] Built target llama-batched-bench
[ 56%] Building CXX object examples/batched/CMakeFiles/llama-batched.dir/batched.cpp.o
[ 56%] Linking CXX executable ../../bin/llama-batched
[ 56%] Built target llama-batched
[ 57%] Building CXX object examples/embedding/CMakeFiles/llama-embedding.dir/embedding.cpp.o
[ 57%] Linking CXX executable ../../bin/llama-embedding
[ 57%] Built target llama-embedding
[ 58%] Building CXX object examples/eval-callback/CMakeFiles/llama-eval-callback.dir/eval-callback.cpp.o
[ 58%] Linking CXX executable ../../bin/llama-eval-callback
[ 58%] Built target llama-eval-callback
[ 59%] Building CXX object examples/gbnf-validator/CMakeFiles/llama-gbnf-validator.dir/gbnf-validator.cpp.o
[ 59%] Linking CXX executable ../../bin/llama-gbnf-validator
[ 59%] Built target llama-gbnf-validator
[ 59%] Building C object examples/gguf-hash/CMakeFiles/sha256.dir/deps/sha256/sha256.c.o
[ 59%] Built target sha256
[ 60%] Building C object examples/gguf-hash/CMakeFiles/xxhash.dir/deps/xxhash/xxhash.c.o
[ 60%] Built target xxhash
[ 61%] Building C object examples/gguf-hash/CMakeFiles/sha1.dir/deps/sha1/sha1.c.o
[ 61%] Built target sha1
[ 61%] Building CXX object examples/gguf-hash/CMakeFiles/llama-gguf-hash.dir/gguf-hash.cpp.o
[ 62%] Linking CXX executable ../../bin/llama-gguf-hash
[ 62%] Built target llama-gguf-hash
[ 62%] Building CXX object examples/gguf-split/CMakeFiles/llama-gguf-split.dir/gguf-split.cpp.o
[ 63%] Linking CXX executable ../../bin/llama-gguf-split
[ 63%] Built target llama-gguf-split
[ 63%] Building CXX object examples/gguf/CMakeFiles/llama-gguf.dir/gguf.cpp.o
[ 64%] Linking CXX executable ../../bin/llama-gguf
[ 64%] Built target llama-gguf
[ 64%] Building CXX object examples/gritlm/CMakeFiles/llama-gritlm.dir/gritlm.cpp.o
[ 65%] Linking CXX executable ../../bin/llama-gritlm
[ 65%] Built target llama-gritlm
[ 65%] Building CXX object examples/imatrix/CMakeFiles/llama-imatrix.dir/imatrix.cpp.o
[ 66%] Linking CXX executable ../../bin/llama-imatrix
[ 66%] Built target llama-imatrix
[ 66%] Building CXX object examples/infill/CMakeFiles/llama-infill.dir/infill.cpp.o
[ 67%] Linking CXX executable ../../bin/llama-infill
[ 67%] Built target llama-infill
[ 68%] Building CXX object examples/llama-bench/CMakeFiles/llama-bench.dir/llama-bench.cpp.o
[ 68%] Linking CXX executable ../../bin/llama-bench
[ 68%] Built target llama-bench
[ 68%] Building CXX object examples/lookahead/CMakeFiles/llama-lookahead.dir/lookahead.cpp.o
[ 69%] Linking CXX executable ../../bin/llama-lookahead
[ 69%] Built target llama-lookahead
[ 69%] Building CXX object examples/lookup/CMakeFiles/llama-lookup.dir/lookup.cpp.o
[ 70%] Linking CXX executable ../../bin/llama-lookup
[ 70%] Built target llama-lookup
[ 70%] Building CXX object examples/lookup/CMakeFiles/llama-lookup-create.dir/lookup-create.cpp.o
[ 71%] Linking CXX executable ../../bin/llama-lookup-create
[ 71%] Built target llama-lookup-create
[ 71%] Building CXX object examples/lookup/CMakeFiles/llama-lookup-merge.dir/lookup-merge.cpp.o
[ 72%] Linking CXX executable ../../bin/llama-lookup-merge
[ 72%] Built target llama-lookup-merge
[ 72%] Building CXX object examples/lookup/CMakeFiles/llama-lookup-stats.dir/lookup-stats.cpp.o
[ 73%] Linking CXX executable ../../bin/llama-lookup-stats
[ 73%] Built target llama-lookup-stats
[ 74%] Building CXX object examples/main/CMakeFiles/llama-cli.dir/main.cpp.o
[ 74%] Linking CXX executable ../../bin/llama-cli
[ 74%] Built target llama-cli
[ 74%] Building CXX object examples/parallel/CMakeFiles/llama-parallel.dir/parallel.cpp.o
[ 74%] Linking CXX executable ../../bin/llama-parallel
[ 74%] Built target llama-parallel
[ 75%] Building CXX object examples/passkey/CMakeFiles/llama-passkey.dir/passkey.cpp.o
[ 75%] Linking CXX executable ../../bin/llama-passkey
[ 75%] Built target llama-passkey
[ 76%] Building CXX object examples/perplexity/CMakeFiles/llama-perplexity.dir/perplexity.cpp.o
[ 76%] Linking CXX executable ../../bin/llama-perplexity
[ 76%] Built target llama-perplexity
[ 77%] Building CXX object examples/quantize/CMakeFiles/llama-quantize.dir/quantize.cpp.o
[ 77%] Linking CXX executable ../../bin/llama-quantize
[ 77%] Built target llama-quantize
[ 78%] Building CXX object examples/retrieval/CMakeFiles/llama-retrieval.dir/retrieval.cpp.o
[ 78%] Linking CXX executable ../../bin/llama-retrieval
[ 78%] Built target llama-retrieval
[ 79%] Generating loading.html.hpp
[ 79%] Generating index.html.gz.hpp
[ 79%] Building CXX object examples/server/CMakeFiles/llama-server.dir/server.cpp.o
[ 80%] Linking CXX executable ../../bin/llama-server
[ 80%] Built target llama-server
[ 80%] Building CXX object examples/save-load-state/CMakeFiles/llama-save-load-state.dir/save-load-state.cpp.o
[ 81%] Linking CXX executable ../../bin/llama-save-load-state
[ 81%] Built target llama-save-load-state
[ 82%] Building CXX object examples/run/CMakeFiles/llama-run.dir/run.cpp.o
[ 82%] Building CXX object examples/run/CMakeFiles/llama-run.dir/linenoise.cpp/linenoise.cpp.o
[ 83%] Linking CXX executable ../../bin/llama-run
[ 83%] Built target llama-run
[ 83%] Building CXX object examples/simple/CMakeFiles/llama-simple.dir/simple.cpp.o
[ 83%] Linking CXX executable ../../bin/llama-simple
[ 83%] Built target llama-simple
[ 84%] Building CXX object examples/simple-chat/CMakeFiles/llama-simple-chat.dir/simple-chat.cpp.o
[ 84%] Linking CXX executable ../../bin/llama-simple-chat
[ 84%] Built target llama-simple-chat
[ 85%] Building CXX object examples/speculative/CMakeFiles/llama-speculative.dir/speculative.cpp.o
[ 85%] Linking CXX executable ../../bin/llama-speculative
[ 85%] Built target llama-speculative
[ 86%] Building CXX object examples/speculative-simple/CMakeFiles/llama-speculative-simple.dir/speculative-simple.cpp.o
[ 86%] Linking CXX executable ../../bin/llama-speculative-simple
[ 86%] Built target llama-speculative-simple
[ 87%] Building CXX object examples/tokenize/CMakeFiles/llama-tokenize.dir/tokenize.cpp.o
[ 87%] Linking CXX executable ../../bin/llama-tokenize
[ 87%] Built target llama-tokenize
[ 88%] Building CXX object examples/tts/CMakeFiles/llama-tts.dir/tts.cpp.o
[ 88%] Linking CXX executable ../../bin/llama-tts
[ 88%] Built target llama-tts
[ 89%] Building CXX object examples/gen-docs/CMakeFiles/llama-gen-docs.dir/gen-docs.cpp.o
[ 89%] Linking CXX executable ../../bin/llama-gen-docs
[ 89%] Built target llama-gen-docs
[ 90%] Building CXX object examples/convert-llama2c-to-ggml/CMakeFiles/llama-convert-llama2c-to-ggml.dir/convert-llama2c-to-ggml.cpp.o
[ 90%] Linking CXX executable ../../bin/llama-convert-llama2c-to-ggml
[ 90%] Built target llama-convert-llama2c-to-ggml
[ 91%] Building CXX object examples/cvector-generator/CMakeFiles/llama-cvector-generator.dir/cvector-generator.cpp.o
[ 91%] Linking CXX executable ../../bin/llama-cvector-generator
[ 91%] Built target llama-cvector-generator
[ 92%] Building CXX object examples/export-lora/CMakeFiles/llama-export-lora.dir/export-lora.cpp.o
[ 92%] Linking CXX executable ../../bin/llama-export-lora
[ 92%] Built target llama-export-lora
[ 93%] Building CXX object examples/quantize-stats/CMakeFiles/llama-quantize-stats.dir/quantize-stats.cpp.o
[ 93%] Linking CXX executable ../../bin/llama-quantize-stats
[ 93%] Built target llama-quantize-stats
[ 94%] Building CXX object examples/llava/CMakeFiles/llava.dir/llava.cpp.o
[ 94%] Building CXX object examples/llava/CMakeFiles/llava.dir/clip.cpp.o
[ 94%] Built target llava
[ 94%] Linking CXX static library libllava_static.a
[ 94%] Built target llava_static
[ 95%] Linking CXX shared library ../../bin/libllava_shared.dylib
[ 95%] Built target llava_shared
[ 95%] Building CXX object examples/llava/CMakeFiles/llama-llava-cli.dir/llava-cli.cpp.o
[ 96%] Linking CXX executable ../../bin/llama-llava-cli
[ 96%] Built target llama-llava-cli
[ 96%] Building CXX object examples/llava/CMakeFiles/llama-minicpmv-cli.dir/minicpmv-cli.cpp.o
[ 97%] Linking CXX executable ../../bin/llama-minicpmv-cli
[ 97%] Built target llama-minicpmv-cli
[ 98%] Building CXX object examples/llava/CMakeFiles/llama-qwen2vl-cli.dir/qwen2vl-cli.cpp.o
[ 98%] Linking CXX executable ../../bin/llama-qwen2vl-cli
[ 98%] Built target llama-qwen2vl-cli
[ 99%] Building CXX object pocs/vdot/CMakeFiles/llama-vdot.dir/vdot.cpp.o
[ 99%] Linking CXX executable ../../bin/llama-vdot
[ 99%] Built target llama-vdot
[100%] Building CXX object pocs/vdot/CMakeFiles/llama-q8dot.dir/q8dot.cpp.o
[100%] Linking CXX executable ../../bin/llama-q8dot
[100%] Built target llama-q8dot
```

### [build llama.cpp for macos](https://medium.com/@nks1608/building-llama-cpp-for-macos-on-intel-silicon-956bcb5b384b)

* `brew install cmake git libomp vulkan-headers glslang molten-vk shaderc vulkan-tools`
* unable to link `vulkan` library with explicit commands
* commands:

```sh
cmake -B build -DLLAMA_CURL=1 -DGGML_METAL=OFF -DGGML_VULKAN=1 \
-DVulkan_INCLUDE_DIR="$(brew --prefix)/opt/vulkan-headers/include" \
-DVulkan_LIBRARY="$(find $(brew --prefix)/lib -name "libvulkan*" -type l)" \
-DOpenMP_ROOT="$(brew --prefix)/opt/libomp" \
-DVulkan_GLSLC_EXECUTABLE="$(brew --prefix)/opt/shaderc/bin/glslc" \
-DVulkan_GLSLANG_VALIDATOR_EXECUTABLE="$(brew --prefix)/opt/glslang/bin/glslangValidator"
cmake --build build --config Release
```

* output:

```sh
-- The C compiler identification is AppleClang 16.0.0.16000026
-- The CXX compiler identification is AppleClang 16.0.0.16000026
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Library/Developer/CommandLineTools/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Library/Developer/CommandLineTools/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found Git: /usr/local/bin/git (found version "2.48.1")
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- ccache found, compilation results will be cached. Disable with GGML_CCACHE=OFF.
-- CMAKE_SYSTEM_PROCESSOR: x86_64
-- Including CPU backend
-- Accelerate framework found
-- Found OpenMP_C: -Xclang -fopenmp (found version "5.1")
-- Found OpenMP_CXX: -Xclang -fopenmp (found version "5.1")
-- Found OpenMP: TRUE (found version "5.1")
-- x86 detected
-- Adding CPU backend variant ggml-cpu: -march=native 
-- Looking for dgemm_
-- Looking for dgemm_ - found
-- Found BLAS: /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/System/Library/Frameworks/Accelerate.framework
-- BLAS found, Libraries: /Library/Developer/CommandLineTools/SDKs/MacOSX15.2.sdk/System/Library/Frameworks/Accelerate.framework
-- BLAS found, Includes: 
-- Including BLAS backend
# problem with locating Vulkan Library
CMake Error at /usr/local/share/cmake/Modules/FindPackageHandleStandardArgs.cmake:233 (message):
  Could NOT find Vulkan (missing: Vulkan_LIBRARY) (found version "1.4.306")
Call Stack (most recent call first):
  /usr/local/share/cmake/Modules/FindPackageHandleStandardArgs.cmake:603 (_FPHSA_FAILURE_MESSAGE)
  /usr/local/share/cmake/Modules/FindVulkan.cmake:595 (find_package_handle_standard_args)
  ggml/src/ggml-vulkan/CMakeLists.txt:4 (find_package)


-- Configuring incomplete, errors occurred!
make: Makefile: No such file or directory
make: *** No rule to make target `Makefile'.  Stop.
```
