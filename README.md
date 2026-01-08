**Quick story :**

I saw a youtube short about Peaky blinders accents. The accent was a bit hard to grasp. then I found the exact video but this time with subtitles. Then I remember an article about how VLC devs are trying to add automated local subtitle generation into VLC media player. but upon further research I found the public version of VLC does not have a built-in feature for automated local subtitle generation yet. 

So I looked for ways to transcribe videos, Then found Whisper, a multilingual transcription model by OpenAI. and it was open sourced. Crazy, right? openAI actually released an open-source model (this was before they released GPT-OSS). and it supported SRT format out of the box. so I tried to run it locally (base model) it works for english and some other languages but lacks accuracy in other more niche languages. For better accuracy I had to use the medium model in google-colab (running locally without a dedicated GPU was out of the question). It worked at some extent
 

But as it was an open-source model, open-source community did its job and made a CPP (C++) port of the model, optimised for faster and accurate transcription.      

[Github repo of whisper.cpp](https://github.com/ggml-org/whisper.cpp)

![whisper](assets/image.png)




## The transcription

This is the video I used as a reference, as it has subtitles, so I can cross-reference it:

[![Peaky Blinders Accents](https://img.youtube.com/vi/D2W5jHN1rvM/0.jpg)](https://www.youtube.com/watch?v=D2W5jHN1rvM)


## Installing Whisper.cpp (Debian/Ubuntu Linux) :

Requirements: 
- git
- CMake
- C/C++ Compiler (GCC)   


**For Debian/Ubuntu Linux, you can install the build tools with:**
```bash
sudo apt update
sudo apt install git build-essential cmake

```
**Clone the Repo**

```bash
git clone https://github.com/ggerganov/whisper.cpp
# open cloned directory
cd whisper.cpp
```

**Download the Model:**  

This is the model that Whisper will use to transcribe.   
Many models are available but I used the `small` model, as it has the perfect balance between speed and accuracy.

```bash
bash ./models/download-ggml-model.sh small
```
This script will download the model file to the ./models directory.

**Build the Project**

```bash
# Go the the root directory and then run 
make
```
it will compile executable binary file into `whisper.cpp/build/bin/` dirrectory. 
and you are ready to use Whisper.cpp

## Usage :

```bash
# in root directory of the project (Whisper.cpp) run this

./build/bin/whisper-cli -m models/ggml-small.bin -f '../Audio/Peaky blinders accents.mp3' 

# ./build/bin/whisper-cli -m [model path] -f [audio_file_path]

```

**Output :**

```bash
whisper_init_from_file_with_params_no_state: loading model from 'models/ggml-small.bin'
whisper_init_with_params_no_state: use gpu    = 1
whisper_init_with_params_no_state: flash attn = 1
whisper_init_with_params_no_state: gpu_device = 0
whisper_init_with_params_no_state: dtw        = 0
whisper_init_with_params_no_state: devices    = 1
whisper_init_with_params_no_state: backends   = 1
whisper_model_load: loading model
whisper_model_load: n_vocab       = 51865
whisper_model_load: n_audio_ctx   = 1500
whisper_model_load: n_audio_state = 768
whisper_model_load: n_audio_head  = 12
whisper_model_load: n_audio_layer = 12
whisper_model_load: n_text_ctx    = 448
whisper_model_load: n_text_state  = 768
whisper_model_load: n_text_head   = 12
whisper_model_load: n_text_layer  = 12
whisper_model_load: n_mels        = 80
whisper_model_load: ftype         = 1
whisper_model_load: qntvr         = 0
whisper_model_load: type          = 3 (small)
whisper_model_load: adding 1608 extra tokens
whisper_model_load: n_langs       = 99
whisper_model_load:          CPU total size =   487.01 MB
whisper_model_load: model size    =  487.01 MB
whisper_backend_init_gpu: device 0: CPU (type: 0)
whisper_backend_init_gpu: no GPU found
whisper_init_state: kv self size  =   18.87 MB
whisper_init_state: kv cross size =   56.62 MB
whisper_init_state: kv pad  size  =    4.72 MB
whisper_init_state: compute buffer (conv)   =   22.42 MB
whisper_init_state: compute buffer (encode) =   33.85 MB
whisper_init_state: compute buffer (cross)  =    6.20 MB
whisper_init_state: compute buffer (decode) =   97.28 MB

system_info: n_threads = 4 / 4 | WHISPER : COREML = 0 | OPENVINO = 0 | CPU : SSE3 = 1 | SSSE3 = 1 | AVX = 1 | AVX2 = 1 | F16C = 1 | FMA = 1 | BMI2 = 1 | OPENMP = 1 | REPACK = 1 | 

main: processing '../Audio/Peaky blinders accents.mp3' (186241 samples, 11.6 sec), 4 threads, 1 processors, 5 beams + best of 5, lang = en, task = transcribe, timestamps = 1 ...

# from here output starts 
[00:00:00.000 --> 00:00:03.800]   [Door opens and closes] # it also detect sounds
[00:00:03.800 --> 00:00:06.600]   John, you caught Angel Changretta.
[00:00:06.600 --> 00:00:11.600]   And now, I've got an Italian walker around my backyard, saying he's gonna kill my brother.

whisper_print_timings:     load time =   389.02 ms
whisper_print_timings:     fallbacks =   0 p /   0 h
whisper_print_timings:      mel time =    36.62 ms
whisper_print_timings:   sample time =   302.85 ms /   228 runs (     1.33 ms per run)
whisper_print_timings:   encode time = 13177.24 ms /     1 runs ( 13177.24 ms per run)
whisper_print_timings:   decode time =    27.27 ms /     1 runs (    27.27 ms per run)
whisper_print_timings:   batchd time =  4271.11 ms /   225 runs (    18.98 ms per run)
whisper_print_timings:   prompt time =     0.00 ms /     1 runs (     0.00 ms per run)
whisper_print_timings:    total time = 18438.57 ms

```
