# AMD_AI_Benchmark_LLM

Setup
Activate the Ryzen AI 1.4 Conda environment:

conda activate ryzen-ai-1.4.0
Copy the required files in a local folder to run the LLMs from:

mkdir hybrid_run
cd hybrid_run
xcopy /Y /E "%RYZEN_AI_INSTALLATION_PATH%\hybrid-llm\onnxruntime_genai\benchmark" .
xcopy /Y "%RYZEN_AI_INSTALLATION_PATH%\hybrid-llm\examples\amd_genai_prompt.txt" .
xcopy /Y "%RYZEN_AI_INSTALLATION_PATH%\deployment\hybrid-llm\onnxruntime-genai.dll" .
xcopy /Y "%RYZEN_AI_INSTALLATION_PATH%\deployment\hybrid-llm\onnx_custom_ops.dll" .
xcopy /Y "%RYZEN_AI_INSTALLATION_PATH%\deployment\hybrid-llm\ryzen_mm.dll" .
xcopy /Y "%RYZEN_AI_INSTALLATION_PATH%\deployment\hybrid-llm\ryzenai_onnx_utils.dll" .
xcopy /Y "%RYZEN_AI_INSTALLATION_PATH%\deployment\voe\DirectML.dll" .
xcopy /Y "%RYZEN_AI_INSTALLATION_PATH%\deployment\voe\onnxruntime.dll" .
Download Models from HuggingFace
Download the desired models from the list of pre-optimized models on Hugging Face:

# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
git clone <link to hf model>
For example, for Llama-2-7b-chat:

git lfs install
git clone https://huggingface.co/amd/Llama-2-7b-chat-hf-awq-g128-int4-asym-fp16-onnx-hybrid
Enabling Performance Mode (Optional)
To run the LLMs in the best performance mode, follow these steps:

Go to Windows → Settings → System → Power and set the power mode to Best Performance.

Execute the following commands in the terminal:

cd C:\Windows\System32\AMD
xrt-smi configure --pmode performance
Sample C++ Program
The model_benchmark.exe test application provides a simple mechanism for running and evaluating Hybrid OGA models using the native OGA C++ APIs. The source code for this application can be used a reference implementation for how to integrate LLMs using the native OGA C++ APIs.

# The model_benchmark.exe test application can be used as follows:

To see available options and default settings
.\model_benchmark.exe -h

To run with default settings
.\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file -l $list_of_prompt_lengths

To show more informational output
.\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file --verbose

To run with given number of generated tokens
.\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file -l $list_of_prompt_lengths -g $num_tokens

To run with given number of warmup iterations
.\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file -l $list_of_prompt_lengths -w $num_warmup

To run with given number of iterations
.\model_benchmark.exe -i $path_to_model_dir  -f $prompt_file -l $list_of_prompt_lengths -r $num_iterations
For example, for Llama-2-7b-chat:

.\model_benchmark.exe -i Llama-2-7b-chat-hf-awq-g128-int4-asym-fp16-onnx-hybrid -f amd_genai_prompt.txt -l "1024" --verbose

NOTE: The C++ source code for the model_benchmark.exe executable can be found in the %RYZEN_AI_INSTALLATION_PATH%\hybrid-llm\examples\c folder. This source code can be modified and recompiled if necessary using the commands below.


(ryzen-ai-1.4.0) C:\Users\Administrator\Desktop\hybrid_run>.\model_benchmark.exe -i DeepSeek-R1-Distill-Qwen-1.5B-awq-asym-uint4-g128-lmhead-onnx-hybrid -f amd_genai_prompt.txt -l "1024" --verbose
