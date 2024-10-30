# llama.vim

Local LLM-assisted text completion.

<img width="485" alt="image" src="https://github.com/user-attachments/assets/a950e38c-3b3f-4c46-94fe-0d6e0f790fc6">

---

![llama vim-swift](https://github.com/user-attachments/assets/206c8399-ff73-495d-ba67-65725138c021)

## Features

- Auto-suggest on cursor movement in `Insert` mode
- Toggle the suggestion manually by pressing `Ctrl+F`
- Accept a suggestion with `Tab`
- Accept the first line of a suggestion with `Shift+Tab`
- Control max text generation time
- Configure scope of context around the cursor
- Ring context with chunks from open and edited files and yanked text
- [Supports very large contexts even on low-end hardware via smart context reuse](https://github.com/ggerganov/llama.cpp/pull/9787)
- Display performance stats

## Installation

### vim-plug

```vim
Plug 'ggml-org/llama.vim'
```

The plugin requires a [llama.cpp](https://github.com/ggerganov/llama.cpp) server instance to be running at [`g:llama_config.endpoint`](https://github.com/ggml-org/llama.vim/blob/7d3359077adbad4c05872653973c3ceb09f18ad9/autoload/llama.vim#L34-L36)

### Mac OS

```bash
brew install llama.cpp
```

### Any other OS

Either build from source or use the latest binaries: https://github.com/ggerganov/llama.cpp/releases

### llama.cpp settings

Here are recommended settings, depending on the amount of VRAM that you have:

- More than 16GB VRAM:

  ```bash
  llama-server \
      --hf-repo ggml-org/Qwen2.5-Coder-7B-Q8_0-GGUF \
      --hf-file qwen2.5-coder-7b-q8_0.gguf \
      --port 8012 -ngl 99 -fa -ub 1024 -b 1024 -dt 0.1 \
      --cache-reuse 256
  ```

- Less than 16GB VRAM:

  ```bash
  llama-server \
      --hf-repo ggml-org/Qwen2.5-Coder-1.5B-Q8_0-GGUF \
      --hf-file qwen2.5-coder-1.5b-q8_0.gguf \
      --port 8012 -ngl 99 -fa -ub 1024 -b 1024 -dt 0.1 \
      --cache-reuse 256
  ```

### Recommended LLMs

The plugin requires FIM-compatible models: [HF collection](https://huggingface.co/collections/ggml-org/llamavim-6720fece33898ac10544ecf9)

## Examples

<img width="1758" alt="image" src="https://github.com/user-attachments/assets/8f5748b3-183a-4b7f-90e1-9148f0a58883">

### Using `llama.vim` on M1 Pro (2021) with `Qwen2.5-Coder 1.5B Q8_0`:

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/0ccb93c6-c5c5-4376-a5a3-cc99fafc5eef">

The orange text is the generated suggestion. The green text contains performance stats for the FIM request: the currently used context is `15186` tokens and the maximum is `32768`. There are `30` chunks in the ring buffer with extra context (out of `64`). So far, `1` chunk has been evicted in the current session and there are `0` chunks in queue. The newly computed prompt tokens for this request were `260` and the generated tokens were `25`. It took `1245 ms` to generate this suggestion after entering the letter `c` on the current line.

### Using `llama.vim` on M2 Ultra with `Qwen2.5-Coder 7B Q8_0`:

https://github.com/user-attachments/assets/1f1eb408-8ac2-4bd2-b2cf-6ab7d6816754

Demonstrates that the global context is accumulated and maintained across different files and showcases the overall latency when working in a large codebase.
