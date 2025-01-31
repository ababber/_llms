# sample output

## `./build/bin/llama-cli -m ../llm-models/gemma-1.1-7b-it-Q4_K_M-GGUF/gemma-1.1-7b-it.Q4_K_M.gguf`

```sh
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = AMD Radeon RX 6900 XT (MoltenVK) | uma: 0 | fp16: 1 | warp size: 64 | matrix cores: none
build: 4589 (eb7cf15a) with Apple clang version 16.0.0 (clang-1600.0.26.6) for x86_64-apple-darwin23.6.0
main: llama backend init
main: load the model and apply lora adapter, if any
llama_model_load_from_file_impl: using device Vulkan0 (AMD Radeon RX 6900 XT) - 16368 MiB free
llama_model_loader: loaded meta data with 24 key-value pairs and 254 tensors from ../llm-models/gemma-1.1-7b-it-Q4_K_M-GGUF/gemma-1.1-7b-it.Q4_K_M.gguf (version GGUF V3 (latest))
llama_model_loader: Dumping metadata keys/values. Note: KV overrides do not apply in this output.
llama_model_loader: - kv   0:                       general.architecture str              = gemma
llama_model_loader: - kv   1:                               general.name str              = gemma-1.1-7b-it
llama_model_loader: - kv   2:                       gemma.context_length u32              = 8192
llama_model_loader: - kv   3:                     gemma.embedding_length u32              = 3072
llama_model_loader: - kv   4:                          gemma.block_count u32              = 28
llama_model_loader: - kv   5:                  gemma.feed_forward_length u32              = 24576
llama_model_loader: - kv   6:                 gemma.attention.head_count u32              = 16
llama_model_loader: - kv   7:              gemma.attention.head_count_kv u32              = 16
llama_model_loader: - kv   8:     gemma.attention.layer_norm_rms_epsilon f32              = 0.000001
llama_model_loader: - kv   9:                 gemma.attention.key_length u32              = 256
llama_model_loader: - kv  10:               gemma.attention.value_length u32              = 256
llama_model_loader: - kv  11:                          general.file_type u32              = 15
llama_model_loader: - kv  12:                       tokenizer.ggml.model str              = llama
llama_model_loader: - kv  13:                      tokenizer.ggml.tokens arr[str,256000]  = { "<pad>", "<eos>", "<bos>", "<unk>", "..." }
llama_model_loader: - kv  14:                      tokenizer.ggml.scores arr[f32,256000]  = { 0.000000, 0.000000, 0.000000, 0.0000, "..." }
llama_model_loader: - kv  15:                  tokenizer.ggml.token_type arr[i32,256000]  = { 3, 3, 3, 2, 1, 1, 1, 1, 1, 1, 1, 1, "..." }
llama_model_loader: - kv  16:                tokenizer.ggml.bos_token_id u32              = 2
llama_model_loader: - kv  17:                tokenizer.ggml.eos_token_id u32              = 1
llama_model_loader: - kv  18:            tokenizer.ggml.unknown_token_id u32              = 3
llama_model_loader: - kv  19:            tokenizer.ggml.padding_token_id u32              = 0
llama_model_loader: - kv  20:               tokenizer.ggml.add_bos_token bool             = true
llama_model_loader: - kv  21:               tokenizer.ggml.add_eos_token bool             = false
llama_model_loader: - kv  22:                    tokenizer.chat_template str              = {{ bos_token }}{% if messages[0]['rol...']}
llama_model_loader: - kv  23:               general.quantization_version u32              = 2
llama_model_loader: - type  f32:   57 tensors
llama_model_loader: - type q4_K:  168 tensors
llama_model_loader: - type q6_K:   29 tensors
print_info: file format = GGUF V3 (latest)
print_info: file type   = Q4_K - Medium
print_info: file size   = 4.96 GiB (4.99 BPW) 
load: control-looking token:    107 '<end_of_turn>' was not control-type; this is probably a bug in the model. its type will be overridden
load: special_eos_id is not in special_eog_ids - the tokenizer config may be incorrect
load: special tokens cache size = 5
load: token to piece cache size = 1.6014 MB
print_info: arch             = gemma
print_info: vocab_only       = 0
print_info: n_ctx_train      = 8192
print_info: n_embd           = 3072
print_info: n_layer          = 28
print_info: n_head           = 16
print_info: n_head_kv        = 16
print_info: n_rot            = 256
print_info: n_swa            = 0
print_info: n_embd_head_k    = 256
print_info: n_embd_head_v    = 256
print_info: n_gqa            = 1
print_info: n_embd_k_gqa     = 4096
print_info: n_embd_v_gqa     = 4096
print_info: f_norm_eps       = 0.0e+00
print_info: f_norm_rms_eps   = 1.0e-06
print_info: f_clamp_kqv      = 0.0e+00
print_info: f_max_alibi_bias = 0.0e+00
print_info: f_logit_scale    = 0.0e+00
print_info: n_ff             = 24576
print_info: n_expert         = 0
print_info: n_expert_used    = 0
print_info: causal attn      = 1
print_info: pooling type     = 0
print_info: rope type        = 2
print_info: rope scaling     = linear
print_info: freq_base_train  = 10000.0
print_info: freq_scale_train = 1
print_info: n_ctx_orig_yarn  = 8192
print_info: rope_finetuned   = unknown
print_info: ssm_d_conv       = 0
print_info: ssm_d_inner      = 0
print_info: ssm_d_state      = 0
print_info: ssm_dt_rank      = 0
print_info: ssm_dt_b_c_rms   = 0
print_info: model type       = 7B
print_info: model params     = 8.54 B
print_info: general.name     = gemma-1.1-7b-it
print_info: vocab type       = SPM
print_info: n_vocab          = 256000
print_info: n_merges         = 0
print_info: BOS token        = 2 '<bos>'
print_info: EOS token        = 1 '<eos>'
print_info: EOT token        = 107 '<end_of_turn>'
print_info: UNK token        = 3 '<unk>'
print_info: PAD token        = 0 '<pad>'
print_info: LF token         = 227 '<0x0A>'
print_info: EOG token        = 1 '<eos>'
print_info: EOG token        = 107 '<end_of_turn>'
print_info: max token length = 93
load_tensors: offloading 0 repeating layers to GPU
load_tensors: offloaded 0/29 layers to GPU
load_tensors:   CPU_Mapped model buffer size =  5077.09 MiB
llama_init_from_model: n_seq_max     = 1
llama_init_from_model: n_ctx         = 4096
llama_init_from_model: n_ctx_per_seq = 4096
llama_init_from_model: n_batch       = 2048
llama_init_from_model: n_ubatch      = 512
llama_init_from_model: flash_attn    = 0
llama_init_from_model: freq_base     = 10000.0
llama_init_from_model: freq_scale    = 1
llama_init_from_model: n_ctx_per_seq (4096) < n_ctx_train (8192) -- the full capacity of the model will not be utilized
llama_kv_cache_init: kv_size = 4096, offload = 1, type_k = 'f16', type_v = 'f16', n_layer = 28, can_shift = 1
llama_kv_cache_init:        CPU KV buffer size =  1792.00 MiB
llama_init_from_model: KV self size  = 1792.00 MiB, K (f16):  896.00 MiB, V (f16):  896.00 MiB
llama_init_from_model:        CPU  output buffer size =     0.98 MiB
llama_init_from_model:    Vulkan0 compute buffer size =    18.00 MiB
llama_init_from_model:        CPU compute buffer size =   506.00 MiB
llama_init_from_model: Vulkan_Host compute buffer size =   144.01 MiB
llama_init_from_model: graph nodes  = 931
llama_init_from_model: graph splits = 508 (with bs=512), 1 (with bs=1)
common_init_from_params: setting dry_penalty_last_n to ctx_size = 4096
common_init_from_params: warming up the model with an empty run - please wait ... (--no-warmup to disable)
main: llama threadpool init, n_threads = 4
main: chat template is available, enabling conversation mode (disable it with -no-cnv)
main: chat template example:
<start_of_turn>user
You are a helpful assistant

Hello<end_of_turn>
<start_of_turn>model
Hi there<end_of_turn>
<start_of_turn>user
How are you?<end_of_turn>
<start_of_turn>model


system_info: n_threads = 4 (n_threads_batch = 4) / 8 | CPU : SSE3 = 1 | SSSE3 = 1 | AVX = 1 | AVX2 = 1 | F16C = 1 | FMA = 1 | AVX512 = 1 | AVX512_VBMI = 1 | AVX512_VNNI = 1 | LLAMAFILE = 1 | ACCELERATE = 1 | OPENMP = 1 | AARCH64_REPACK = 1 | 

main: interactive mode on.
sampler seed: 2156464723
sampler params: 
 repeat_last_n = 64, repeat_penalty = 1.000, frequency_penalty = 0.000, presence_penalty = 0.000
 dry_multiplier = 0.000, dry_base = 1.750, dry_allowed_length = 2, dry_penalty_last_n = 4096
 top_k = 40, top_p = 0.950, min_p = 0.050, xtc_probability = 0.000, xtc_threshold = 0.100, typical_p = 1.000, temp = 0.800
 mirostat = 0, mirostat_lr = 0.100, mirostat_ent = 5.000
sampler chain: logits -> logit-bias -> penalties -> dry -> top-k -> typical -> top-p -> min-p -> xtc -> temp-ext -> dist 
generate: n_ctx = 4096, n_batch = 2048, n_predict = -1, n_keep = 1

== Running in interactive mode. ==
 - Press Ctrl+C to interject at any time.
 - Press Return to return control to the AI.
 - To return control without starting a new line, end your input with '/'.
 - If you want to submit another line, end your input with '\'.
 - Using default system message. To change it, set a different value via -p PROMPT or -f FILE argument.


> Tell me about yourself
"I am an AI assistant designed to provide helpful and informative responses to user inquiries. I have been trained on a vast dataset of information and language patterns, enabling me to understand and respond to diverse requests.

**Here are some key aspects of my abilities:**

* **Natural Language Processing (NLP):** I can understand and generate human language, allowing me to provide meaningful responses to textual inputs.
* **Knowledge Base:** I have access to a vast repository of knowledge and information on various topics.
* **Reasoning and Problem-Solving:** I can analyze information, identify patterns, and solve problems based on the context of the conversation.
* **Learning and Adaptation:** I continue to learn and improve my abilities over time, based on the interactions and feedback I receive.

**I can assist you with:**

* Providing information and answers to your questions
* Completing tasks and providing guidance
* Generating creative and informative content
* Translating languages and simplifying complex concepts

I am committed to being a helpful and reliable assistant, and I am constantly striving to enhance my abilities and provide the best possible user experience."

> 
llama_perf_sampler_print:    sampling time =      50.10 ms /   257 runs   (    0.19 ms per token,  5129.54 tokens per second)
llama_perf_context_print:        load time =    2089.71 ms
llama_perf_context_print: prompt eval time =   12166.26 ms /    32 tokens (  380.20 ms per token,     2.63 tokens per second)
llama_perf_context_print:        eval time =   34177.54 ms /   225 runs   (  151.90 ms per token,     6.58 tokens per second)
llama_perf_context_print:       total time =   54488.85 ms /   257 tokens
Interrupted by user
```

## `./build/bin/llama-cli -m ../llm-models/Meta-Llama-3.1-8B-Instruct-Q4_0-GGUF/meta-llama-3.1-8b-instruct-q4_0.gguf`

```sh
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = AMD Radeon RX 6900 XT (MoltenVK) | uma: 0 | fp16: 1 | warp size: 64 | matrix cores: none
build: 4589 (eb7cf15a) with Apple clang version 16.0.0 (clang-1600.0.26.6) for x86_64-apple-darwin23.6.0
main: llama backend init
main: load the model and apply lora adapter, if any
llama_model_load_from_file_impl: using device Vulkan0 (AMD Radeon RX 6900 XT) - 16368 MiB free
llama_model_loader: loaded meta data with 30 key-value pairs and 292 tensors from ../llm-models/Meta-Llama-3.1-8B-Instruct-Q4_0-GGUF/meta-llama-3.1-8b-instruct-q4_0.gguf (version GGUF V3 (latest))
llama_model_loader: Dumping metadata keys/values. Note: KV overrides do not apply in this output.
llama_model_loader: - kv   0:                       general.architecture str              = llama
llama_model_loader: - kv   1:                               general.type str              = model
llama_model_loader: - kv   2:                               general.name str              = Meta Llama 3.1 8B Instruct
llama_model_loader: - kv   3:                           general.finetune str              = Instruct
llama_model_loader: - kv   4:                           general.basename str              = Meta-Llama-3.1
llama_model_loader: - kv   5:                         general.size_label str              = 8B
llama_model_loader: - kv   6:                            general.license str              = llama3.1
llama_model_loader: - kv   7:                               general.tags arr[str,6]       = { "facebook", "meta", "pytorch", "llam..." }
llama_model_loader: - kv   8:                          general.languages arr[str,8]       = { "en", "de", "fr", "it", "pt", "hi", ... }
llama_model_loader: - kv   9:                          llama.block_count u32              = 32
llama_model_loader: - kv  10:                       llama.context_length u32              = 131072
llama_model_loader: - kv  11:                     llama.embedding_length u32              = 4096
llama_model_loader: - kv  12:                  llama.feed_forward_length u32              = 14336
llama_model_loader: - kv  13:                 llama.attention.head_count u32              = 32
llama_model_loader: - kv  14:              llama.attention.head_count_kv u32              = 8
llama_model_loader: - kv  15:                       llama.rope.freq_base f32              = 500000.000000
llama_model_loader: - kv  16:     llama.attention.layer_norm_rms_epsilon f32              = 0.000010
llama_model_loader: - kv  17:                          general.file_type u32              = 2
llama_model_loader: - kv  18:                           llama.vocab_size u32              = 128256
llama_model_loader: - kv  19:                 llama.rope.dimension_count u32              = 128
llama_model_loader: - kv  20:             llama.rope.scaling.attn_factor f32              = 1.000000
llama_model_loader: - kv  21:                       tokenizer.ggml.model str              = gpt2
llama_model_loader: - kv  22:                         tokenizer.ggml.pre str              = llama-bpe
llama_model_loader: - kv  23:                      tokenizer.ggml.tokens arr[str,128256]  = { "!", "\"", "#", "$", "%", "&", "\'", ... }
llama_model_loader: - kv  24:                  tokenizer.ggml.token_type arr[i32,128256]  = { 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, ... }
llama_model_loader: - kv  25:                      tokenizer.ggml.merges arr[str,280147]  = { "Ġ Ġ", "Ġ ĠĠĠ", "ĠĠ ĠĠ", "..." }
llama_model_loader: - kv  26:                tokenizer.ggml.bos_token_id u32              = 128000
llama_model_loader: - kv  27:                tokenizer.ggml.eos_token_id u32              = 128009
llama_model_loader: - kv  28:                    tokenizer.chat_template str              = {% set loop_messages = messages %}{% ...}
llama_model_loader: - kv  29:               general.quantization_version u32              = 2
llama_model_loader: - type  f32:   66 tensors
llama_model_loader: - type  f16:    2 tensors
llama_model_loader: - type q4_0:  224 tensors
print_info: file format = GGUF V3 (latest)
print_info: file type   = Q4_0
print_info: file size   = 5.61 GiB (6.01 BPW) 
load: special tokens cache size = 256
load: token to piece cache size = 0.7999 MB
print_info: arch             = llama
print_info: vocab_only       = 0
print_info: n_ctx_train      = 131072
print_info: n_embd           = 4096
print_info: n_layer          = 32
print_info: n_head           = 32
print_info: n_head_kv        = 8
print_info: n_rot            = 128
print_info: n_swa            = 0
print_info: n_embd_head_k    = 128
print_info: n_embd_head_v    = 128
print_info: n_gqa            = 4
print_info: n_embd_k_gqa     = 1024
print_info: n_embd_v_gqa     = 1024
print_info: f_norm_eps       = 0.0e+00
print_info: f_norm_rms_eps   = 1.0e-05
print_info: f_clamp_kqv      = 0.0e+00
print_info: f_max_alibi_bias = 0.0e+00
print_info: f_logit_scale    = 0.0e+00
print_info: n_ff             = 14336
print_info: n_expert         = 0
print_info: n_expert_used    = 0
print_info: causal attn      = 1
print_info: pooling type     = 0
print_info: rope type        = 0
print_info: rope scaling     = linear
print_info: freq_base_train  = 500000.0
print_info: freq_scale_train = 1
print_info: n_ctx_orig_yarn  = 131072
print_info: rope_finetuned   = unknown
print_info: ssm_d_conv       = 0
print_info: ssm_d_inner      = 0
print_info: ssm_d_state      = 0
print_info: ssm_dt_rank      = 0
print_info: ssm_dt_b_c_rms   = 0
print_info: model type       = 8B
print_info: model params     = 8.03 B
print_info: general.name     = Meta Llama 3.1 8B Instruct
print_info: vocab type       = BPE
print_info: n_vocab          = 128256
print_info: n_merges         = 280147
print_info: BOS token        = 128000 <|begin_of_text|>
print_info: EOS token        = 128009 '<|eot_id|>'
print_info: EOT token        = 128009 '<|eot_id|>'
print_info: EOM token        = 128008 '<|eom_id|>'
print_info: LF token         = 128 'Ä'
print_info: EOG token        = 128008 '<|eom_id|>'
print_info: EOG token        = 128009 '<|eot_id|>'
print_info: max token length = 256
load_tensors: offloading 0 repeating layers to GPU
load_tensors: offloaded 0/33 layers to GPU
load_tensors:  CPU_AARCH64 model buffer size =  3744.00 MiB
load_tensors:   CPU_Mapped model buffer size =  5749.02 MiB
llama_init_from_model: n_seq_max     = 1
llama_init_from_model: n_ctx         = 4096
llama_init_from_model: n_ctx_per_seq = 4096
llama_init_from_model: n_batch       = 2048
llama_init_from_model: n_ubatch      = 512
llama_init_from_model: flash_attn    = 0
llama_init_from_model: freq_base     = 500000.0
llama_init_from_model: freq_scale    = 1
llama_init_from_model: n_ctx_per_seq (4096) < n_ctx_train (131072) -- the full capacity of the model will not be utilized
llama_kv_cache_init: kv_size = 4096, offload = 1, type_k = 'f16', type_v = 'f16', n_layer = 32, can_shift = 1
llama_kv_cache_init:        CPU KV buffer size =   512.00 MiB
llama_init_from_model: KV self size  =  512.00 MiB, K (f16):  256.00 MiB, V (f16):  256.00 MiB
llama_init_from_model:        CPU  output buffer size =     0.49 MiB
llama_init_from_model:    Vulkan0 compute buffer size =    24.00 MiB
llama_init_from_model:        CPU compute buffer size =   258.50 MiB
llama_init_from_model: Vulkan_Host compute buffer size =   288.01 MiB
llama_init_from_model: graph nodes  = 1030
llama_init_from_model: graph splits = 196 (with bs=512), 1 (with bs=1)
common_init_from_params: setting dry_penalty_last_n to ctx_size = 4096
common_init_from_params: warming up the model with an empty run - please wait ... (--no-warmup to disable)
main: llama threadpool init, n_threads = 4
main: chat template is available, enabling conversation mode (disable it with -no-cnv)
main: chat template example:
<|start_header_id|>system<|end_header_id|>

You are a helpful assistant<|eot_id|><|start_header_id|>user<|end_header_id|>

Hello<|eot_id|><|start_header_id|>assistant<|end_header_id|>

Hi there<|eot_id|><|start_header_id|>user<|end_header_id|>

How are you?<|eot_id|><|start_header_id|>assistant<|end_header_id|>



system_info: n_threads = 4 (n_threads_batch = 4) / 8 | CPU : SSE3 = 1 | SSSE3 = 1 | AVX = 1 | AVX2 = 1 | F16C = 1 | FMA = 1 | AVX512 = 1 | AVX512_VBMI = 1 | AVX512_VNNI = 1 | LLAMAFILE = 1 | ACCELERATE = 1 | OPENMP = 1 | AARCH64_REPACK = 1 | 

main: interactive mode on.
sampler seed: 2248165188
sampler params: 
 repeat_last_n = 64, repeat_penalty = 1.000, frequency_penalty = 0.000, presence_penalty = 0.000
 dry_multiplier = 0.000, dry_base = 1.750, dry_allowed_length = 2, dry_penalty_last_n = 4096
 top_k = 40, top_p = 0.950, min_p = 0.050, xtc_probability = 0.000, xtc_threshold = 0.100, typical_p = 1.000, temp = 0.800
 mirostat = 0, mirostat_lr = 0.100, mirostat_ent = 5.000
sampler chain: logits -> logit-bias -> penalties -> dry -> top-k -> typical -> top-p -> min-p -> xtc -> temp-ext -> dist 
generate: n_ctx = 4096, n_batch = 2048, n_predict = -1, n_keep = 1

== Running in interactive mode. ==
 - Press Ctrl+C to interject at any time.
 - Press Return to return control to the AI.
 - To return control without starting a new line, end your input with '/'.
 - If you want to submit another line, end your input with '\'.
 - Using default system message. To change it, set a different value via -p PROMPT or -f FILE argument.

system

You are a helpful assistant


> Tell me about yourself
"I'm happy to introduce myself.

I am a helpful assistant, designed to assist and provide information to users like you. I'm a computer program, powered by artificial intelligence (AI) and machine learning (ML) algorithms, which enable me to understand and respond to a wide range of questions and topics.

Here are some things I can do:

1. **Answer questions**: I can process natural language queries and provide relevant answers, drawing from a vast knowledge base.
2. **Provide information**: I can share facts, definitions, and explanations on various subjects, from science and history to entertainment and culture.
3. **Offer suggestions**: If you're looking for ideas or recommendations, I can provide suggestions on topics like travel, food, movies, and more.
4. **Assist with tasks**: I can help with tasks like language translation, text summarization, and even generating ideas.
5. **Chat and converse**: I can engage in conversations, using my understanding of language to respond to your thoughts and ideas.

I'm constantly learning and improving, thanks to the vast amounts of data and feedback I receive from users like you. My goal is to be a helpful and informative assistant, and I strive to do so in a neutral and respectful manner.

How can I assist you today?"

>  
llama_perf_sampler_print:    sampling time =      30.00 ms /   272 runs   (    0.11 ms per token,  9066.67 tokens per second)
llama_perf_context_print:        load time =   30496.09 ms
llama_perf_context_print: prompt eval time =    9977.13 ms /    24 tokens (  415.71 ms per token,     2.41 tokens per second)
llama_perf_context_print:        eval time =   35473.60 ms /   258 runs   (  137.49 ms per token,     7.27 tokens per second)
llama_perf_context_print:       total time =   56181.96 ms /   282 tokens
Interrupted by user
```
