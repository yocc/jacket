# 总结

[toc]



## Overview

prompt, 对模型“编程”, 说明或者示例.

token, 粗略经验法则, 英文, 1token 约等于 4 字符 或者 0.75 单词. prompt + completion < 模型规定的最大上下文长度. 不太常见的词通常被分解为多个token. 有些token以空格开头.

completion endpoint = API 接口. prompt + completion API = text completion

instruction，指令. 指令种类:

1. 说明(prompt)
2. 说明 + 形容词
3. 说明 + 示例
4. 说明 + 示例 + 微调(prompt + examples + fine-tune)

model > GPT-4 > gpt-4, gpt-4-0314, 等

GPT-4, GPT-3, 差距主要是在复杂推理能力上



## TBC

分词工具, token + probability





## OPEN AI

```python
# 1. 翻墙, 前置准备
# 2. 安装 openai pip包
(grpc02) [chenchen@grpc01 grpc02]$ pip install openai
Collecting openai
  Downloading openai-0.27.7-py3-none-any.whl (71 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 72.0/72.0 kB 340.4 kB/s eta 0:00:00
Requirement already satisfied: requests>=2.20 in ./lib/python3.9/site-packages (from openai) (2.30.0)
Collecting tqdm (from openai)
  Downloading tqdm-4.65.0-py3-none-any.whl (77 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 77.1/77.1 kB 770.8 kB/s eta 0:00:00
Collecting aiohttp (from openai)
  Downloading aiohttp-3.8.4-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.0/1.0 MB 1.5 MB/s eta 0:00:00
Requirement already satisfied: charset-normalizer<4,>=2 in ./lib/python3.9/site-packages (from requests>=2.20->openai) (3.1.0)
Requirement already satisfied: idna<4,>=2.5 in ./lib/python3.9/site-packages (from requests>=2.20->openai) (3.4)
Requirement already satisfied: urllib3<3,>=1.21.1 in ./lib/python3.9/site-packages (from requests>=2.20->openai) (1.26.15)
Requirement already satisfied: certifi>=2017.4.17 in ./lib/python3.9/site-packages (from requests>=2.20->openai) (2023.5.7)
Collecting attrs>=17.3.0 (from aiohttp->openai)
  Downloading attrs-23.1.0-py3-none-any.whl (61 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 61.2/61.2 kB 3.2 MB/s eta 0:00:00
Collecting multidict<7.0,>=4.5 (from aiohttp->openai)
  Downloading multidict-6.0.4-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (114 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 114.2/114.2 kB 2.1 MB/s eta 0:00:00
Collecting async-timeout<5.0,>=4.0.0a3 (from aiohttp->openai)
  Downloading async_timeout-4.0.2-py3-none-any.whl (5.8 kB)
Collecting yarl<2.0,>=1.0 (from aiohttp->openai)
  Downloading yarl-1.9.2-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (269 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 269.4/269.4 kB 2.8 MB/s eta 0:00:00
Collecting frozenlist>=1.1.1 (from aiohttp->openai)
  Downloading frozenlist-1.3.3-cp39-cp39-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl (158 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 158.8/158.8 kB 3.7 MB/s eta 0:00:00
Collecting aiosignal>=1.1.2 (from aiohttp->openai)
  Downloading aiosignal-1.3.1-py3-none-any.whl (7.6 kB)
Installing collected packages: tqdm, multidict, frozenlist, attrs, async-timeout, yarl, aiosignal, aiohttp, openai
Successfully installed aiohttp-3.8.4 aiosignal-1.3.1 async-timeout-4.0.2 attrs-23.1.0 frozenlist-1.3.3 multidict-6.0.4 openai-0.27.7 tqdm-4.65.0 yarl-1.9.2
(grpc02) [chenchen@grpc01 grpc02]$ 
# 3. 按文档步骤, 下载示例应用
(grpc02) [chenchen@grpc01 openai]$ git clone https://github.com/openai/openai-quickstart-python.git
# 4. 按文档步骤, 切换到刚下载好的示例应用的目录下
(grpc02) [chenchen@grpc01 openai]$ cd openai-quickstart-python/
# 5. 用 .env.example 文件为模版, 复制一个 .env 文件, 并修改其中的实际 OPENAI_API_KEY
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ cp .env.example .env
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ vim .env
# 6. 按文档步骤, 需求文件中列表所列的包
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ pip install -r requirements.txt
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
tensorflow 2.12.0 requires numpy<1.24,>=1.22, but you have numpy 1.21.3 which is incompatible.
Successfully installed Flask-2.0.2 Jinja2-3.0.2 MarkupSafe-2.0.1 Werkzeug-2.0.2 autopep8-1.6.0 certifi-2021.10.8 charset-normalizer-2.0.7 click-8.0.3 et-xmlfile-1.1.0 idna-3.3 itsdangerous-2.0.1 numpy-1.21.3 openai-0.19.0 openpyxl-3.0.9 pandas-1.3.4 pandas-stubs-1.2.0.35 pycodestyle-2.8.0 python-dateutil-2.8.2 python-dotenv-0.19.2 pytz-2021.3 requests-2.26.0 toml-0.10.2 tqdm-4.62.3 urllib3-1.26.7
# 7. 检查 pip 安装, 有提示 numpy 版本不对
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ pip check
tensorflow 2.12.0 has requirement numpy<1.24,>=1.22, but you have numpy 1.21.3.
# 8. 查看已经安装的 numpy 的具体情况
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ pip show numpy
Name: numpy
Version: 1.21.3
Summary: NumPy is the fundamental package for array computing with Python.
Home-page: https://www.numpy.org
Author: Travis E. Oliphant et al.
Author-email: 
License: BSD
Location: /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages
Requires: 
Required-by: h5py, jax, ml-dtypes, opt-einsum, pandas, scipy, tensorboard, tensorflow, torchvision
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ 
# 9. 升级 numpy 到指定版本. 直接复制 pip check 返回的内容加上双引号即可
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ pip install -U "numpy<1.24,>=1.22"
Collecting numpy<1.24,>=1.22
  Using cached numpy-1.23.5-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (17.1 MB)
Installing collected packages: numpy
  Attempting uninstall: numpy
    Found existing installation: numpy 1.21.3
    Uninstalling numpy-1.21.3:
      Successfully uninstalled numpy-1.21.3
Successfully installed numpy-1.23.5
# 10. 等安装结束后, 继续 pip check, 发现一个错都没有了
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ pip check
No broken requirements found.
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ whereis flask
flask: /home/chenchen/tmp/venv/python39/grpc02/bin/flask
# 11. 按文档步骤, 执行 flask 框架, 是个前台占用的 web + 代码
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ flask run
 * Serving Flask app 'app' (lazy loading)
 * Environment: development
 * Debug mode: on
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 123-416-094
  

# 12. 按文档前面的提示, 测试一下命令行下的指令和返回. 由于是在 shell 命令行执行, 需要 ~/.bashrc 里增加 OPENAI_API_KEY
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ openai api completions.create -m text-davinci-003 -p "Say this is a test" -t 0 -M 7 --stream
Logging requires wandb to be installed. Run `pip install wandb`.
Error: No API key provided. You can set your API key in code using 'openai.api_key = <API-KEY>', or you can set the environment variable OPENAI_API_KEY=<API-KEY>). If your API key is stored in a file, you can point the openai module at it with 'openai.api_key_path = <PATH>'. You can generate API keys in the OpenAI web interface. See https://onboard.openai.com for details, or email support@openai.com if you have any questions.
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ vim .env
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ pip install wandb
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ pip check
No broken requirements found.

# 什么是 python 的 .env 文件?
# 环境变量文件是一个文本文件, 您可以通 过在项目文件夹中将其命名为“.env”来使用它. 这是一种保存您想要使用但又不想在互联网上出现的全局变量的简单方法. 
# 首先,我们需要一些包来让 Python 与 .env 文件一起工作。它们是:dotenv 和 os。
# 我们不想提交 .env 文件。这将导致我们的秘密变量公开。这就是为什么我们要创建一个 .gitignore 文件。
.env 文件
FLASK_APP=app
FLASK_ENV=development

# Once you add your API key below, make sure to not share it with anyone! The API key should remain private.
OPENAI_API_KEY=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

openai.api_key = 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


(grpc02) [chenchen@grpc01 openai-quickstart-python]$ vim ~/.bashrc
# OpenAI
export OPENAI_API_KEY=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


# 13. 检查 OPENAI_API_KEY 是否已经加载入环境变量里了
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ env | grep KEY
OPENAI_API_KEY=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
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ openai api completions.create -m text-davinci-003 -p "Say this is a test" -t 0 -M 7 --stream
# 14. 429错误
Error: You exceeded your current quota, please check your plan and billing details. (HTTP status code: 429)
(grpc02) [chenchen@grpc01 openai-quickstart-python]$ 
```





## FAQ













## See Also















