# GOT-OCR-2 - GUI

## [中文版看这里](README.md)

![img.png](img.png)

Logs are being localized, but there are too many keys, it's hard for me to check if there are any missing keys. If you
Encountered KeyErrors, please report an Issue.

## About this project

Model
weights: [Mirror Site](https://hf-mirror.com/stepfun-ai/GOT-OCR2_0), [Original Site](https://huggingface.co/stepfun-ai/GOT-OCR2_0)  
Original GitHub: [GOT-OCR2.0](https://github.com/Ucas-HaoranWei/GOT-OCR2.0/) 
This project was developed under Windows, so I can't guarantee that it will work on Linux. If you want to use it on
Linux, you can check this [issue](https://github.com/XJF2332/GOT-OCR-2-GUI/issues/3)   
I used [GLM4](https://chatglm.cn/main/alltoolsdetail?lang=zh) and [Deepseek](https://www.deepseek.com/) to generate some of my codes.

Click a star, please

## To-Dos

- [x] Localize logs
- [ ] Add support for new model: stepfun-ai/GOT-OCR-2.0-hf  
- [ ] Optimize PDF error handling
- [ ] Support for `llama-cpp-python`, hoping to accelerate inference
- I ran into some troubles, if you have the solution, you can create a pull request, pull into `Alpha` branch for now;
  HuggingFace model used is [kaifeise/GOT-gguf](https://huggingface.co/kaifeise/GOT-gguf/tree/main); `GGUF Test.py` used
  codes from [1694439208/GOT-OCR-Inference](https://github.com/1694439208/GOT-OCR-Inference). Files are in the `gguf`
  folder
- [ ] html to word functionality, preserve formulas for editing
- I got two ideas for now. One is creating docx from PDF files, but it's too slow since you need selenium to open your
  browser and output PDFs. The other is `pdflatex`, but you have to have latex installed. I checked TexLive, but it's
  too big, so I'm not sure if it's worth it. Is anyone interested in this?

## How to use

If you don't have the folder mentioned here, **create a new one**

### Dependencies

This environment was tested under **python 3.11.9**.

#### torch

Choose a suitable **GPU version** of `torch` and from [PyTorch](https://pytorch.org/get-started/locally/) and install
it.  
I am using stable 2.4.1 + cu124, so I suggest you to use this version.

#### FlashAttention

Not a must-have, but if you want to install it, check [#12](https://github.com/XJF2332/GOT-OCR-2-GUI/issues/12)

#### PyMuPDF

I have tested that if you install it directly through `requiremtns.txt`, then you will get
`ModuleNotFoundError: No module named 'frontend'` error. But if you install it separately in commandline, it will work
fine. I don't know the reason why, just try it yourself.
By the way, if you still get the `ModuleNotFoundError`, try to uninstall and reinstall `fitz` and `PyMuPDF` separately.
I have tested that `pip install -U` won't work. Strange.

```commandline
pip install fitz
pip install PyMuPDF
```

#### Use `pip` to install

```commandline
pip install -r requirements.txt
```

And, someone said that he encountered conflicting dependencies after installing. But I didn't find any conflicting
dependencies in the `requirements.txt` file, and `pipdeptree` shows that nothing is conflicting. I used `pip freeze` to
create this `requirements.txt` file, so it should be fine.  
However, this problem really happened, so I provided a `requirements-noversion.txt` that doesn't contain version
numbers.
For more information, see this [issue #4](https://github.com/XJF2332/GOT-OCR-2-GUI/issues/4)

```commandline
pip install -r requirements-noversion.txt
```

#### Other

- [Edge WebDriver](https://developer.microsoft.com/zh-cn/microsoft-edge/tools/webdriver/?form=MA13LH#downloads),
  download the `.zip` file, unzip it and put it into `msedgedriver` folder

> This requires the Edge browser to be installed on your computer, which is preinstalled on Windows.  
> The file structure should be:
> ```
> GOT-OCR-2-GUI
> └─edge_driver
>    ├─msedgedriver.exe
>    └─...
> ```

### Download model file

1. Download to `models` folder
2. Stop downloading fewer files

- The file structure should be:

```
GOT-OCR-2-GUI
└─models
   ├─config.json
   ├─generation_config.json
   ├─got_vision_b.py
   ├─model.safetensors
   ├─modeling_GOT.py
   ├─qwen.tiktoken
   ├─render_tools.py
   ├─special_tokens_map.json
   ├─tokenization_qwen.py
   └─tokenizer_config.json
```

### Getting Started

1. If you want to use the command line, then use `CLI.py`.
2. If you want to use the graphical interface, then use `GUI.py`.
3. If you want to modify settings, then use `Config Manager.py`.
4. If you want to perform automated rendering operations, then use `Renderer.py`, which will automatically render all
   `.jpg` and `.png` images in the `imgs` folder.

> Those using the GUI can ignore this, but for those using the CLI, remember to place the images you want to OCR into
> the `imgs` folder (the CLI currently only detects `.jpg` and `.png` files).

## Localization Support

- You can find various language `.json` files in the `Locales` folder, with CLI and GUI language files stored
  separately.
- In the `gui` subfolder, in addition to the `language.json` file, there is also an `instructions` folder that contains
  the built-in tutorials for the GUI, named as `language.md`.
- To modify language support, simply change the value of `'language'` in the `config.json` file. The available options
  correspond to the file names without extensions in the `language.json` files.
- If you wish to add language support, for the CLI, just add a new `language.json` file (I strongly recommend using an
  existing file as a starting point). For the GUI, you will also need the corresponding `language.md` file.
- You can run `Config Manager.py` to manage the language and other configuration files.

## Error Codes

| Error Code | Message                                     |
|------------|---------------------------------------------|
| 1          | Config file not found                       |
| 2          | Language file not found                     |
| 3          | CLI do not have support for folder          |
| 4          | Input file not found                        |
| 5          | Unsupported file type                       |
| 6          | Failed to detect encoding of rendered HTMLs |

## Tips

- DO NOT DELETE `markdown-it.js` inside `result` folder, or pdf outputting may fail

> If you deleted it, you can find a backup in `scripts` folder. Just copy it to `result` folder.

- If the script crashes, you can try running `cmd` with `python + file name`, I encountered crashes during testing, but
  I don't know why
- Make sure hat you installed the gpu version of `torch`

## Common Questions

- Q: CLI.py: error: the following arguments are required: --path/-P
- A: Use PowerShell instead, cmd has this problem and I can't find the reason.
---
- Q: What is an "HTML local file"? Are there HTML files that are not saved locally?
- A: Although the HTML files output by the model are saved locally, they use external scripts. Therefore, even if the
  file is on your local machine, you still need an internet connection to open it. I have downloaded the external
  script, which is the previously mentioned `markdown-it.js`. The main reason for doing this is to prevent PDF export
  failures due to network issues.
---
- Q: Why did my model fail to load?
- A: Check if you are missing any files. It seems that the model files downloaded from Baidu Cloud are missing some
  files. I recommend you download from the previously mentioned Huggingface instead.
---
- Q：Any suggestions on deploying this repo?
- A: See this [issue #5](https://github.com/XJF2332/GOT-OCR-2-GUI/issues/5)
---
- Q: Where can I read the document?
- A: For GUI users, you can navigate to `instruction` tab. For CLI users, you can run `.\CLI.py --help` to read documents automatically generated by argparse, or run `.\CLI.py --detailed-help` to read a more detailed version.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=XJF2332/GOT-OCR-2-GUI&type=Date)](https://star-history.com/#XJF2332/GOT-OCR-2-GUI&Date)