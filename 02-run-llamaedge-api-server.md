# Run llamaedge api server application

## What you will learn

* The way to use and develop an OpenAI compatible API server
* How to use WasmEdge+WASI-NN to run the API server with the different models

## Configure your environment

* Install WasmEdge with the wasi-nn-ggml plugin:

```
# install WasmEdge with wasi-nn-ggml plugin
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash -s -- --plugin wasi_nn-ggml
# If your environment doesn't have python, use the following command to install the plugin
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install_v2.sh | bash
```

The installer will check the operating system and download the corresponding WasmEdge binary release. The installer will also download the wasi-nn-ggml plugin and install it.

Note: The installer only support Linux and MacOS. If you are using Windows, please use WSL or WSL2.

## Get the Chat application

```
curl -LO https://github.com/LlamaEdge/LlamaEdge/releases/latest/download/llama-api-server.wasm
```

You can also get the repository from [here](https://github.com/LlamaEdge/LlamaEdge/tree/main/api-server) and build from source by yourself.

## Get LLM

```
# download model
curl -LO https://huggingface.co/second-state/Llama-2-7B-Chat-GGUF/resolve/main/llama-2-7b-chat.Q5_K_M.gguf
```

## Run the model with WasmEdge
```
# run the `llama-api-server` wasm app with the model
wasmedge --dir .:. \
    --nn-preload default:GGML:AUTO:llama-2-7b-chat.Q5_K_M.gguf \
    llama-api-server.wasm \
    --prompt-template llama-2-chat \
    --model-name llama-2-7b-chat
```

## Interact with the API server

### `/v1/models` endpoint

```
curl -X POST http://localhost:8080/v1/models -H 'accept:application/json'
```

### Other enpoints

See: https://github.com/LlamaEdge/LlamaEdge/tree/main/api-server#endpoints
