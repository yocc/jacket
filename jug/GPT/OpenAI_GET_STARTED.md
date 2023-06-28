# OpenAI

-----

[toc]





## [Introduction](https://platform.openai.com/docs/introduction/introduction)

### [Overview](https://platform.openai.com/docs/introduction/overview)

```
Looking for ChatGPT? Head to [chat.openai.com](https://chat.openai.com/).
```

OpenAI API 几乎可以应用于任何涉及理解或生成自然语言、代码或图像的任务。我们提供一系列[模型]（https://platform.openai.com/docs/models），具有适合不同任务的不同功率水平，以及[微调]（https://platform.openai.com/docs/guides/fine 调整）您自己的自定义模型的能力。这些模型可用于从内容生成到语义搜索和分类的所有内容。

 提供了一系列模型model（针对不同功率与之相配的不同任务）和对此模型model的微调fine-tune

suitable，相配的

as well as，以及

fine-tune，微调

classification，分类



### [Key concepts](https://platform.openai.com/docs/introduction/key-concepts) 关键概念

我们建议完成快速入门教程，通过动手的交互式示例熟悉关键概念。

acquainted，了解，熟悉

```
Quickstart tutorial
Learn by building a quick sample application
```

#### [Prompts](https://platform.openai.com/docs/introduction/prompts) 提示

[Designing your prompt](https://platform.openai.com/docs/guides/completion/prompt-design) is essentially how you “program” the model, usually by providing some instructions or a few examples. This is different from most other NLP services which are designed for a single task, such as sentiment classification or named entity recognition. Instead, the completions and chat completions endpoint can be used for virtually any task including content or code generation, summarization, expansion, conversation, creative writing, style transfer, and more.

[设计提示]（https://platform.openai.com/docs/guides/completion/prompt-design）本质上是你如何“编程”模型，通常是通过提供一些指令或几个例子。这与大多数其他 NLP 服务不同，后者是为单个任务设计的，例如情绪分类或命名实体识别。相反，补全completion和聊天完成终结点completion endpoint 几乎可用于任何任务，包括内容或代码生成、摘要、扩展、对话、创意写作、风格迁移等。

设计提示就是编程模型的本质。 

#### [Tokens](https://platform.openai.com/docs/introduction/tokens) 令牌

我们的模型通过将文本分解为令牌token来理解和处理文本。令牌token可以是单词，也可以只是字符块。例如，单词“hamburger”被分解为令牌token“ham”，“bur”和“ger”，而像“pear”这样的简短而常见的单词是单个令牌token。许多令牌token以空格开头，例如“hello”和“bye”。

 给定 API 请求中处理的令牌token数量取决于输入和输出的长度。根据粗略的经验法则，对于英文文本，1 个令牌token大约是 4 个字符或 0.75 个单词。要记住的一个限制是，文本提示prompt和生成的补全completion组合不得超过模型的最大上下文长度（对于大多数模型，这是 2048 个token，或大约 1500 个单词）。查看我们的 [分词器工具]（https://platform.openai.com/tokenizer），详细了解文本如何转换为分词。

check out，查看

tokenizer tool，分词工具

 text translates to tokens，文本转换为分词

#### [Models](https://platform.openai.com/docs/introduction/models) 模型

The API is powered by a set of models with different capabilities and price points. GPT-4 is our latest and most powerful model. GPT-3.5-Turbo is the model that powers ChatGPT and is optimized for conversational formats. To learn more about these models and what else we offer, visit our [models documentation](https://platform.openai.com/docs/models).

该 API 由一组具有不同功能capabilities和价位price的模型提供支持。GPT-4 是我们最新、功能最强大的型号。GPT-3.5-Turbo 是支持 ChatGPT 的模型，并针对对话格式进行了优化。要了解有关这些模型以及我们提供的其他功能的更多信息，请访问我们的 [模型文档]（https://platform.openai.com/docs/models）。

optimize，优化

#### [Next steps](https://platform.openai.com/docs/introduction/next-steps) 

- Keep our [usage policies](https://openai.com/policies/usage-policies) in mind as you start building your application. 在开始构建应用程序时，请记住我们的 [使用策略]（https://openai.com/policies/usage-policies）。
- Explore our [examples library](https://platform.openai.com/examples) for inspiration. 探索我们的 [示例库]（https://platform.openai.com/examples）以获取灵感。
- Jump into one of our guides to start building. 跳转到我们的指南之一，开始构建。



##### [Guides](https://platform.openai.com/docs/introduction/guides) 指南

Chat Beta 聊天，Learn how to use chat-based language models 学习如何使用基于聊天的语言模型

Text completion 文本补全，Learn how to generate or edit text 学习如何生成或者编辑文本

Embeddings 嵌入，Learn how to search, classify, and compare text 学习如何搜索，分类，和比较文本

Speech to text Beta 语音到文本，Learn how to turn audio into text 学习如何将音频转换为文本

Image generation Beta 图片生成，Learn how to generate or edit images 学习如何生成或者编辑图片

Fine-tuning 微调，Learn how to train a model for your use case 学习如何为您的用例训练模型 （有用）



## [Quickstart](https://platform.openai.com/docs/quickstart/quickstart)

OpenAI 是被训练出来的非常擅长理解和生成文本的尖端语言模型。我们的 API 提供对这些模型的访问，可用于解决几乎任何涉及处理语言的任务。

在这个快速入门指引里， 您将会构建一个简单的例子。在此过程中，您将学习使用 API 完成任何任务的关键概念和技术，包括：

- 内容生成 Content generation
- 摘要 Summarization
- 分类，归类和情感分析 Classification, categorization, and sentiment analysis
- 数据提取 Data extraction
- 翻译 Translation
- 还有更多 Many more!

### [Introduction](https://platform.openai.com/docs/quickstart/introduction)

补全点 completions endpoint 是我们 API 的核心，它提供了一个非常灵活和强大的简单接口。您输入一些文本作为提示 prompt，API 将返回一个文本补全completion，尝试匹配您为其提供的任何指令instructions或上下文context。

completions endpoint 补全点：接口

completions，补全：给定提示prompt，模型model 将返回一个或多个预测 补全 completions，还可以返回每个位置的替代令牌tokens 的概率 predicted。

model，模型

prompt，提示

instruction，指令

context，上下文

```
Prompt: Write a tagline for an ice cream shop. 写一个冰淇淋商店的标语
Completion: We serve up smiles with every scoop! 我们用每一勺提供微笑！
```

您可以将其视为非常高级的自动完成功能 ——  该模型会处理您的文本提示并尝试预测接下来最有可能出现的内容。

### 1. Start with an instruction 从指令开始

假设您想创建一个宠物名字生成器。从头开始想出名字很难！

首先，您需要一个提示，这个提示明确说明您想要什么。 让我们从一个指令开始。 提交此提示 Submit this prompt 以生成您的第一个补全completion。

```
Suggest one name for a horse.
为一匹马建议一个名字。
```

不错！现在，试着让你的指示prompt 更具体。

```
Suggest one name for a black horse.
为一匹黑马建议一个名字。
```

如您所见，在我们的提示中添加一个简单的形容词adjective 会改变生成的补全。设计您的提示prompt 本质上就是您“编程”模型的方式。

adjective， 形容词

essentially， 本质上

### 2. [Add some examples](https://platform.openai.com/docs/quickstart/add-some-examples) 添加一些例子

制定好的指令对于获得良好的结果很重要，但有时它们还不够。让我们尝试使您的指令更加复杂。

 ```
 Suggest three names for a horse that is a superhero.
 为一匹超级英雄的马建议三个名字。
 ```

这个补全completion 并不是我们想要的。这些名字非常通用generic，似乎模型model 没有接受我们指令中的马部分。让我们看看我们是否可以让它提出一些更相关的建议。

在许多情况下，显示 *and* 告诉模型model 你想要什么很有帮助。在提示prompt 中添加示例有助于传达模式patterns 或细微差别nuances。尝试提交此提示 submitting this prompt，其中包括几个示例。

```
Suggest three names for an animal that is a superhero.
为超级英雄的动物建议三个名字。

Animal: Cat
Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline
Animal: Dog
Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot
Animal: Horse
Names:
```

好！为给定输入添加我们期望的输出示例有助于模型提供我们正在寻找的名称类型。

### 3. [Adjust your settings](https://platform.openai.com/docs/quickstart/adjust-your-settings) 调整您的设置

Prompt design isn’t the only tool you have at your disposal. You can also control completions by adjusting your settings. One of the most important settings is called **temperature**.

提示prompt 设计并不是您可以使用的唯一工具。您还可以通过调整设置来控制补全。最重要的设置之一称为**温度**。

temperature，温度

您可能已经注意到，如果在上面的示例中多次提交相同的提示pormpt，则模型将始终返回相同或非常相似的补全completion。这是因为您的温度temperature 设置为 **0**。

尝试在温度设置为 **1** 的情况下重新提交相同的提示prompt 几次。‍

```
Temperature = 0.5

Suggest three names for an animal that is a superhero.
为超级英雄的动物建议三个名字。

Animal: Cat
Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline
Animal: Dog
Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot
Animal: Horse
Names:
```

看看发生了什么？当温度高于 0 时，提交相同的提示prompt 每次会导致不同的完成。

请记住，该模型预测哪些文本最有可能跟随其前面的文本。温度是介于 0 和 1 之间的值，实质essentially 上允许您控制模型在进行这些预测时的置信度 confident。降低温度意味着风险risks 更小，补全completion 将更加准确accurate 和确定deterministic。温度升高将导致更多样化的补全completion。

essentially， 本质上

confident，置信度

accurate，准确

deterministic，准确

predicts，预测

```
DEEP DIVE 深入讨论
Understanding tokens and probabilities
理解令牌和概率
```

token，令牌

probability，概率

对于您的宠物名字生成器，您可能希望能够生成很多名字创意。0.6的中等温度temperature应该效果很好。

### 4. [Build your application](https://platform.openai.com/docs/quickstart/build-your-application) 构建您的应用

NODE.JS    PYTHON (FLASK)

FLASK，Flask是一个使用[Python](Python.html)编写的轻量级Web应用框架。基于Werkzeug [WSGI](WSGI.html)工具箱和Jinja2 模板引擎。 Flask使用BSD授权。 

Flask也被称为“microframework”，因为它使用简单的核心，用extension增加其他功能。Flask没有缺省使用的数据库、表单验证工具。然而，Flask保留了扩增的弹性，可以用[Flask-extension](http://flask.pocoo.org/extensions/)加入这些功能：[ORM](對象關係映射.html)、表单验证工具、文件上传、各种开放式身份验证技术。

现在您已经找到了一个很好的提示prompt 和设置setting，您可以构建您的宠物名字生成器generator了！我们编写了一些代码来帮助你入门 - 按照以下说明下载代码并运行应用。

generator，生成器

#### [Setup](https://platform.openai.com/docs/quickstart/setup) 设置

如果您没有安装 Node.js，[从这里安装]（https://nodejs.org/en/）。然后通过克隆[此存储库]（https://github.com/openai/openai-快速启动节点）下载代码。

如果您没有安装Python，[从这里安装]（https://www.python.org/downloads/）。然后通过克隆[此存储库]（https://github.com/openai/openai-quickstart-python）下载代码。

```text
git clone https://github.com/openai/openai-quickstart-node.git
git clone https://github.com/openai/openai-quickstart-python.git
```

如果你不想使用 git，你也可以使用 [this zip 文件]（https://github.com/openai/openai-quickstart-python/archive/refs/heads/master.zip）下载代码。

#### [Add your API key](https://platform.openai.com/docs/quickstart/add-your-api-key) 添加您的 API key

导航到项目目录并复制示例环境变量文件。

```text
cd openai-quickstart-node
cp .env.example .env

cd openai-quickstart-python
cp .env.example .env
```

复制您的私有 API 密钥，并将其设置为新创建的“.env”文件中的“OPENAI_API_KEY”。如果您尚未创建密钥，可以在下面执行此操作。  

您目前没有任何 API 密钥。请在下面创建一个。

Create new secret key‍

重要提示：使用 Javascript 时，所有 API 调用都应仅在服务器端进行，因为在客户端浏览器代码中进行调用将暴露您的 API 密钥。[看这里]（https://platform.openai.com/docs/api-reference/authentication）了解更多详情。

#### [Run the app](https://platform.openai.com/docs/quickstart/run-the-app) 运行应用

在项目目录中运行以下命令以安装依赖项并运行应用。

在项目目录中运行以下命令以安装依赖项并运行应用。运行命令时，您可能需要键入“python3”/“pip3”而不是“python”/“pip”，具体取决于您的设置。

```text
npm install
npm run dev

python -m venv venv
. venv/bin/activate
pip install -r requirements.txt
flask run
```

在浏览器中打开[http://localhost:3000]（http://localhost:3000/），您应该会看到宠物名称生成器！

#### [Understand the code](https://platform.openai.com/docs/quickstart/understand-the-code) 理解代码

在“openai-quickstart-node/pages/api”文件夹中打开“生成.js”。在底部，您将看到生成我们上面使用的提示的函数。由于用户将输入其宠物的动物类型，因此它会动态交换提示中指定动物的部分。

在“openai-quickstart-python”文件夹中打开“app.py”。在底部，您将看到生成我们上面使用的提示的函数。由于用户将输入其宠物的动物类型，因此它会动态交换提示中指定动物的部分。

```python
function generatePrompt(animal) {
  const capitalizedAnimal = animal[0].toUpperCase() + animal.slice(1).toLowerCase();
  return `Suggest three names for an animal that is a superhero.

Animal: Cat
Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline
Animal: Dog
Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot
Animal: ${capitalizedAnimal}
Names:`;
}


def generate_prompt(animal):
    return """Suggest three names for an animal that is a superhero.

Animal: Cat
Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline
Animal: Dog
Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot
Animal: {}
Names:""".format(animal.capitalize())
```

在“生成.js”的第 9 行，您将看到发送实际 API 请求的代码。如上所述，它使用温度为 0.6 的 [completions]（https://platform.openai.com/docs/api-reference/completions） 端点。  

在“app.py”的第 14 行，您将看到发送实际 API 请求的代码。如上所述，它使用温度为 0.6 的 [completions]（https://platform.openai.com/docs/api-reference/completions） 端点。

```python
const completion = await openai.createCompletion({
  model: "text-davinci-003",
  prompt: generatePrompt(req.body.animal),
  temperature: 0.6,
});


response = openai.Completion.create(
  model="text-davinci-003",
  prompt=generate_prompt(animal),
  temperature=0.6
)
```

就是这样！您现在应该完全了解您的（超级英雄）宠物名字生成器如何使用 OpenAI API！

#### [Pricing](https://platform.openai.com/docs/quickstart/pricing) 定价

我们提供一系列具有不同功能和[价格点]（https://openai.com/pricing/）的[型号]（https://platform.openai.com/docs/models）。在本教程中，我们使用了“text-davinci-003”。我们建议在实验时使用此模型或“gpt-3.5-turbo”，因为它们会产生最佳结果。一旦事情正常，您可以查看其他模型是否可以以更低的延迟和成本产生相同的结果。或者，如果您可能需要迁移到更强大的模型，例如“gpt-4”。  

单个请求（提示prompt和完成completion）中处理的令牌tokens总数不能超过模型的最大上下文长度maximum context length。对于大多数模型，这是 4，096 个令牌token或大约 3，000 个单词。根据粗略的经验法则，对于英文文本，1 个令牌token大约是 4 个字符或 0.75 个单词。  

定价为每 1，000 个token即用即付，前 3 个月内可以使用 5 美元的免费信用额度。[了解更多]（https://openai.com/pricing/）。

capabilities，功能

#### [Closing](https://platform.openai.com/docs/quickstart/closing) 结语

这些概念和技术将大大有助于您构建自己的应用程序。也就是说，这个简单的例子只展示了可能性的一小部分！补全端点completion endpoint 足够灵活，可以解决几乎任何语言处理任务，包括内容生成、摘要、语义搜索、主题标记、情绪分析等等。

要记住的一个限制是，对于大多数模型，单个 API 请求在提示prompt和补全completion之间最多只能处理 4，096 个令牌tokens。

对于更高级的任务，您可能会发现自己希望能够提供比单个提示prompt 所能容纳的更多的示例examples或上下文context。[微调API]（https://platform.openai.com/docs/guides/fine-tuning）对于此类更高级的任务来说是一个很好的选择。微调允许您提供数百甚至数千个示例examples，以针对您的特定用例自定义模型。

concepts，概念

techniques，术语

semantic，语义的

sentiment，情绪

### [Next steps](https://platform.openai.com/docs/quickstart/next-steps)

要获得灵感并了解有关为不同任务设计提示designing prompts 的更多信息，请执行以下操作：  

- Read our [completion guide](https://platform.openai.com/docs/guides/completion). 阅读我们的[完成指南]（https://platform.openai.com/docs/guides/completion）。
- Explore our library of [example prompts](https://platform.openai.com/examples). 探索我们的 [示例提示]库（https://platform.openai.com/examples）。
- Start experimenting in the [Playground](https://platform.openai.com/playground). 在[游乐场]（https://platform.openai.com/playground）中开始实验。
- Keep our [usage policies](https://openai.com/policies/usage-policies) in mind as you start building. 在开始构建时，请记住我们的 [使用策略]（https://openai.com/policies/usage-policies）。

inspired，灵感

## Libraries

https://platform.openai.com/docs/libraries

## Models

### [Overview](https://platform.openai.com/docs/models/overview)

OpenAI API 由具有不同功能和价位的各种模型提供支持。您还可以通过 [微调]（https://platform.openai.com/docs/guides/fine-tuning）针对您的特定用例对原始基本模型进行有限的自定义。

| MODELS                                                       | DESCRIPTION                                                  |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| [GPT-4](https://platform.openai.com/docs/models/gpt-4) Limited beta | A set of models that improve on GPT-3.5 and can understand as well as generate natural language or code 一组改进 GPT-3.5 的模型，可以理解和生成自然语言或代码 |
| [GPT-3.5](https://platform.openai.com/docs/models/gpt-3-5)   | A set of models that improve on GPT-3 and can understand as well as generate natural language or code 一组改进 GPT-3 的模型，可以理解并生成自然语言或代码 |
| [DALL·E](https://platform.openai.com/docs/models/dall-e) Beta | A model that can generate and edit images given a natural language prompt 可以在给定自然语言提示的情况下生成和编辑图像的模型 |
| [Whisper](https://platform.openai.com/docs/models/whisper) Beta | A model that can convert audio into text 一种可以将音频转换为文本的模型 |
| [Embeddings](https://platform.openai.com/docs/models/embeddings) | A set of models that can convert text into a numerical form 一组可以将文本转换为数字形式的模型 |
| [Moderation](https://platform.openai.com/docs/models/moderation) | A fine-tuned model that can detect whether text may be sensitive or unsafe 可以检测文本是否敏感或不安全的微调模型 |
| [GPT-3](https://platform.openai.com/docs/models/gpt-3)       | A set of models that can understand and generate natural language 一组可以理解和生成自然语言的模型 |
| [Codex](https://platform.openai.com/docs/models/codex) Deprecated | A set of models that can understand and generate code, including translating natural language to code 一组可以理解和生成代码的模型，包括将自然语言转换为代码 |

We have also published open source models including [Point-E](https://github.com/openai/point-e), [Whisper](https://github.com/openai/whisper), [Jukebox](https://github.com/openai/jukebox), and [CLIP](https://github.com/openai/CLIP).

Visit our [model index for researchers](https://platform.openai.com/docs/model-index-for-researchers) to learn more about which models have been featured in our research papers and the differences between model series like InstructGPT and GPT-3.5.

我们还发布了开源模型，包括[Point-E]（https://github.com/openai/point-e），[Whisper]（https://github.com/openai/whisper），[Jukebox]（https://github.com/openai/jukebox）和[CLIP]（https://github.com/openai/CLIP）。

访问我们的[研究人员模型索引]（https://platform.openai.com/docs/model-index-for researchers），以了解有关我们的研究论文中介绍了哪些模型以及InstructGPT和GPT-3.5等模型系列之间的差异的更多信息。

### [Model endpoint compatibility](https://platform.openai.com/docs/models/model-endpoint-compatibility) 模型端点兼容性

| ENDPOINT                 | MODEL NAME                                                   |
| :----------------------- | :----------------------------------------------------------- |
| /v1/chat/completions     | gpt-4, gpt-4-0314, gpt-4-32k, gpt-4-32k-0314, gpt-3.5-turbo, gpt-3.5-turbo-0301 |
| /v1/completions          | text-davinci-003, text-davinci-002, text-curie-001, text-babbage-001, text-ada-001 |
| /v1/edits                | text-davinci-edit-001, code-davinci-edit-001                 |
| /v1/audio/transcriptions | whisper-1                                                    |
| /v1/audio/translations   | whisper-1                                                    |
| /v1/fine-tunes           | davinci, curie, babbage, ada                                 |
| /v1/embeddings           | text-embedding-ada-002, text-search-ada-doc-001              |
| /v1/moderations          | text-moderation-stable, text-moderation-latest               |

此列表不包括我们的[第一代嵌入模型]（https://platform.openai.com/docs/guides/embeddings/similarity-embeddings）或我们的[DALL·E型号]（https://platform.openai.com/docs/guides/images/image-generation-beta）。

compatibility, 兼容性

### [Continuous model upgrades](https://platform.openai.com/docs/models/continuous-model-upgrades) 持续模型升级

随着“gpt-3.5-turbo”的发布，我们的一些型号现在正在不断更新。我们还提供静态模型版本，开发人员可以在引入更新模型后继续使用至少三个月。随着模型更新的新节奏，我们还让人们能够贡献 eval，以帮助我们针对不同的用例改进模型。如果您有兴趣，请查看[OpenAI Evals]（https://github.com/openai/evals）存储库。

以下模型是临时快照，一旦更新版本可用，我们将宣布其弃用日期。如果要使用最新的模型版本，请使用标准模型名称，例如“gpt-4”或“gpt-3.5-turbo”。

| MODEL NAME         | DEPRECATION DATE |
| :----------------- | :--------------- |
| gpt-3.5-turbo-0301 | TBD              |
| gpt-4-0314         | TBD              |
| gpt-4-32k-0314     | TBD              |



### GPT-4 Limited beta

GPT-4 是一个大型多模态模型（今天接受文本输入和发出文本输出，将来会出现图像输入），由于其更广泛的一般知识和高级推理能力，它可以比我们以前的任何模型更准确地解决难题。与“gpt-3.5-turbo”一样，GPT-4 针对聊天进行了优化，但对于使用 [聊天完成 API]（https://platform.openai.com/docs/api-reference/chat ）的传统完成任务效果很好。在我们的 [聊天指南]（https://platform.openai.com/docs/guides/chat） 中了解如何使用 GPT-4。 

GPT-4 目前处于有限的测试阶段，只有那些被授予访问权限的人才能访问。请[加入候补名单]（https://openai.com/waitlist/gpt-4）以便在容量可用时获得访问权限。

| LATEST MODEL   | DESCRIPTION                                                  | MAX TOKENS    | TRAINING DATA  |
| :------------- | :----------------------------------------------------------- | :------------ | :------------- |
| gpt-4          | More capable than any GPT-3.5 model, able to do more complex tasks, and optimized for chat. Will be updated with our latest model iteration. 比任何 GPT-3.5 型号都更强大，能够执行更复杂的任务，并针对聊天进行了优化。将使用我们最新的模型迭代进行更新。 | 8,192 tokens  | Up to Sep 2021 |
| gpt-4-0314     | Snapshot of `gpt-4` from March 14th 2023. Unlike `gpt-4`, this model will not receive updates, and will be deprecated 3 months after a new version is released. 2023 年 3 月 14 日“gpt-4”的快照。与“gpt-4”不同，此模型不会收到更新，并将在新版本发布 3 个月后弃用。 | 8,192 tokens  | Up to Sep 2021 |
| gpt-4-32k      | Same capabilities as the base `gpt-4` mode but with 4x the context length. Will be updated with our latest model iteration. 功能与基本“gpt-4”模式相同，但上下文长度是其 4 倍。将使用我们最新的模型迭代进行更新。 | 32,768 tokens | Up to Sep 2021 |
| gpt-4-32k-0314 | Snapshot of `gpt-4-32` from March 14th 2023. Unlike `gpt-4-32k`, this model will not receive updates, and will be deprecated 3 months after a new version is released. 2023 年 3 月 14 日的“gpt-4-32”快照。与“gpt-4-32k”不同，此模型不会收到更新，并将在新版本发布 3 个月后弃用。 | 32,768 tokens | Up to Sep 2021 |

对于许多基本任务，GPT-4 和 GPT-3.5 模型之间的差异并不显着。但是，在更复杂的推理情况下，GPT-4 比我们以前的任何型号都更有能力。

### [GPT-3.5](https://platform.openai.com/docs/models/gpt-3-5) 

GPT-3.5 模型可以理解和生成自然语言或代码。我们在 GPT-3.5 系列中功能最强大、最具成本效益的型号是“gpt-3.5-turbo”，它已针对聊天进行了优化，但也适用于传统的完成任务。

| LATEST MODEL       | DESCRIPTION                                                  | MAX TOKENS   | TRAINING DATA  |
| :----------------- | :----------------------------------------------------------- | :----------- | :------------- |
| gpt-3.5-turbo      | Most capable GPT-3.5 model and optimized for chat at 1/10th the cost of `text-davinci-003`. Will be updated with our latest model iteration. 功能最强大的 GPT-3.5 型号，针对聊天进行了优化，成本是“text-davinci-003”的 1/10。将使用我们最新的模型迭代进行更新。 | 4,096 tokens | Up to Sep 2021 |
| gpt-3.5-turbo-0301 | Snapshot of `gpt-3.5-turbo` from March 1st 2023. Unlike `gpt-3.5-turbo`, this model will not receive updates, and will be deprecated 3 months after a new version is released. 2023 年 3 月 1 日的“gpt-3.5-turbo”快照。与“gpt-3.5-turbo”不同，此模型不会收到更新，并将在新版本发布 3 个月后弃用。 | 4,096 tokens | Up to Sep 2021 |
| text-davinci-003   | Can do any language task with better quality, longer output, and consistent instruction-following than the curie, babbage, or ada models. Also supports [inserting](https://platform.openai.com/docs/guides/completion/inserting-text) completions within text. 可以完成任何语言任务，质量更好，输出时间更长，并且遵循一致的指令，而不是居里，巴贝奇或ADA模型。还支持在文本中 [插入]（https://platform.openai.com/docs/guides/completion/inserting 文本）补全。 | 4,097 tokens | Up to Jun 2021 |
| text-davinci-002   | Similar capabilities to `text-davinci-003` but trained with supervised fine-tuning instead of reinforcement learning 与“text-davinci-003”类似的功能，但通过监督微调而不是强化学习进行训练 | 4,097 tokens | Up to Jun 2021 |
| code-davinci-002   | Optimized for code-completion tasks 针对代码完成任务进行了优化 | 8,001 tokens | Up to Jun 2021 |

我们建议使用“gpt-3.5-turbo”而不是其他 GPT-3.5 型号，因为它的成本较低。

OpenAI模型是非确定性的，这意味着相同的输入可以产生不同的输出。将 [temperature]（https://platform.openai.com/docs/api-reference/completions/create#completions/create-temperature） 设置为 0 将使输出大部分是确定性的，但可能会保留少量的可变性。

### [Feature-specific models](https://platform.openai.com/docs/models/feature-specific-models) 特定功能模型

虽然新的“gpt-3.5-turbo”模型针对聊天进行了优化，但它非常适合传统的完成任务。原始 GPT-3.5 模型针对 [文本补全]（https://platform.openai.com/docs/guides/completion） 进行了优化。

我们的端点用于[创建嵌入]（https://platform.openai.com/docs/guides/embeddings）和[编辑文本]（https://platform.openai.com/docs/guides/completion/editing-text）使用自己的专用模型集。

### [Finding the right model](https://platform.openai.com/docs/models/finding-the-right-model) 找到正确的模型

尝试使用“gpt-3.5-turbo”是了解 API 能够做什么的好方法。在你对你想要完成的事情有了想法之后，你可以继续使用“gpt-3.5-turbo”或其他模型，并尝试围绕其功能进行优化。

您可以使用 [GPT 比较工具]（https://gpttools.com/comparisontool），该工具允许您并行运行不同的模型来比较输出、设置和响应时间，然后将数据下载到 Excel 电子表格中。

### DALL·E Beta

DALL·E is a AI system that can create realistic images and art from a description in natural language. We currently support the ability, given a prommpt, to create a new image with a certain size, edit an existing image, or create variations of a user provided image.

The current DALL·E model available through our API is the 2nd iteration of DALL·E with more realistic, accurate, and 4x greater resolution images than the original model. You can try it through the our [Labs interface](https://labs.openai.com/) or [via the API](https://platform.openai.com/docs/guides/images/introduction).

DALL·E 是一个AI系统，可以从自然语言的描述中创建逼真的图像和艺术。我们目前支持创建具有特定大小的新图像、编辑现有图像或创建用户提供的图像的变体的功能。  

现在的 DALL·E 通过我们的 API 提供的 E 模型是 DALL· 的第二次迭代E 具有比原始模型更逼真、更准确和分辨率高 4 倍的图像。您可以通过我们的[实验室界面]（https://labs.openai.com/）或[通过API]（https://platform.openai.com/docs/guides/images/introduction）进行尝试。

### Whisper Beta

Whisper is a general-purpose speech recognition model. It is trained on a large dataset of diverse audio and is also a multi-task model that can perform multilingual speech recognition as well as speech translation and language identification. The Whisper v2-large model is currently available through our API with the `whisper-1` model name.

Currently, there is no difference between the [open source version of Whisper](https://github.com/openai/whisper) and the version available through our API. However, through our API, we offer an optimized inference process which makes running Whisper through our API much faster than doing it through other means. For more technical details on Whisper, you can [read the paper](https://arxiv.org/abs/2212.04356).

耳语是一种通用语音识别模型。它是在各种音频的大型数据集上进行训练的，也是一个多任务模型，可以执行多语言语音识别以及语音翻译和语言识别。Whisper v2-large 模型目前可通过我们的 API 使用，模型名称为“whisper-1”。

目前，[Whisper的开源版本]（https://github.com/openai/whisper）和通过我们的API提供的版本之间没有区别。但是，通过我们的 API，我们提供了一个优化的推理过程，这使得通过我们的 API 运行 Whisper 比通过其他方式运行要快得多。有关Whisper的更多技术细节，您可以[阅读论文]（https://arxiv.org/abs/2212.04356）。

### [Embeddings](https://platform.openai.com/docs/models/embeddings)

Embeddings are a numerical representation of text that can be used to measure the relateness between two pieces of text. Our second generation embedding model, `text-embedding-ada-002` is a designed to replace the previous 16 first-generation embedding models at a fraction of the cost. Embeddings are useful for search, clustering, recommendations, anomaly detection, and classification tasks. You can read more about our latest embedding model in the [announcement blog post](https://openai.com/blog/new-and-improved-embedding-model).

嵌入是文本的数字表示形式，可用于衡量两段文本之间的相关性。我们的第二代嵌入模型“text-embedding-ada-002”旨在以一小部分成本取代之前的 16 个第一代嵌入模型。嵌入对于搜索、聚类分析、建议、异常检测和分类任务非常有用。您可以在 [公告博客文章]（https://openai.com/blog/new 和改进的嵌入模型）中阅读有关我们最新嵌入模型的更多信息。

### [Moderation](https://platform.openai.com/docs/models/moderation)  审核模型

The Moderation models are designed to check whether content complies with OpenAI's [usage policies](https://openai.com/policies/usage-policies). The models provide classification capabilities that look for content in the following categories: hate, hate/threatening, self-harm, sexual, sexual/minors, violence, and violence/graphic. You can find out more in our [moderation guide](https://platform.openai.com/docs/guides/moderation/overview).

Moderation models take in an arbitrary sized input that is automatically broken up to fix the models specific context window.

审核模型旨在检查内容是否符合OpenAI的[使用策略]（https://openai.com/policies/usage 策略）。这些模型提供的分类功能可查找以下类别中的内容：仇恨、仇恨/威胁、自残、性、性/未成年人、暴力和暴力/图形。您可以在我们的[审核指南]（https://platform.openai.com/docs/guides/moderation/overview）中找到更多信息。

审核模型采用任意大小的输入，该输入会自动分解以修复模型特定的上下文窗口。

| MODEL                  | DESCRIPTION                                                  |
| :--------------------- | :----------------------------------------------------------- |
| text-moderation-latest | Most capable moderation model. Accuracy will be slighlty higher than the stable model 最有能力的审核模型。精度将比稳定模型高出 |
| text-moderation-stable | Almost as capable as the latest model, but slightly older. 几乎与最新型号一样强大，但略旧。 |

### [GPT-3](https://platform.openai.com/docs/models/gpt-3)

GPT-3 models can understand and generate natural language. These models were superceded by the more powerful GPT-3.5 generation models. However, the original GPT-3 base models (`davinci`, `curie`, `ada`, and `babbage`) are current the only models that are available to fine-tune.

| LATEST MODEL     | DESCRIPTION                                                  | MAX TOKENS   | TRAINING DATA  |
| :--------------- | :----------------------------------------------------------- | :----------- | :------------- |
| text-curie-001   | Very capable, faster and lower cost than Davinci.            | 2,049 tokens | Up to Oct 2019 |
| text-babbage-001 | Capable of straightforward tasks, very fast, and lower cost. | 2,049 tokens | Up to Oct 2019 |
| text-ada-001     | Capable of very simple tasks, usually the fastest model in the GPT-3 series, and lowest cost. | 2,049 tokens | Up to Oct 2019 |
| davinci          | Most capable GPT-3 model. Can do any task the other models can do, often with higher quality. | 2,049 tokens | Up to Oct 2019 |
| curie            | Very capable, but faster and lower cost than Davinci.        | 2,049 tokens | Up to Oct 2019 |
| babbage          | Capable of straightforward tasks, very fast, and lower cost. | 2,049 tokens | Up to Oct 2019 |
| ada              | Capable of very simple tasks, usually the fastest model in the GPT-3 series, and lowest cost. | 2,049 tokens | Up to Oct 2019 |

### Codex Deprecated

The Codex models are now deprecated. They were descendants of our GPT-3 models that would understand and generate code. Their training data contains both natural language and billions of lines of public code from GitHub. [Learn more](https://help.openai.com/en/articles/5480054).

They’re most capable in Python and proficient in over a dozen languages including JavaScript, Go, Perl, PHP, Ruby, Swift, TypeScript, SQL, and even Shell.

The following Codex models are now deprecated:

| LATEST MODEL     | DESCRIPTION                                                  | MAX TOKENS         | TRAINING DATA  |
| :--------------- | :----------------------------------------------------------- | :----------------- | :------------- |
| code-davinci-002 | Most capable Codex model. Particularly good at translating natural language to code. In addition to completing code, also supports [inserting](https://platform.openai.com/docs/guides/code/inserting-code) completions within code. | 8,001 tokens       | Up to Jun 2021 |
| code-davinci-001 | Earlier version of `code-davinci-002`                        | 8,001 tokens       | Up to Jun 2021 |
| code-cushman-002 | Almost as capable as Davinci Codex, but slightly faster. This speed advantage may make it preferable for real-time applications. | Up to 2,048 tokens |                |
| code-cushman-001 | Earlier version of `code-cushman-002`                        | Up to 2,048 tokens |                |

For more, visit our guide on [working with Codex](https://platform.openai.com/docs/guides/code).

## [Tutorials](https://platform.openai.com/docs/tutorials/tutorials) 指引

Get started with the OpenAI API by building real AI apps step by step.

通过逐步构建真正的 AI 应用来开始使用 OpenAI API。

Website Q&A with Embeddings 使用嵌入进行网站问答, Learn how to build an AI that can answer questions about your website. 了解如何构建可以回答有关您网站问题的 AI。

Coming soon 即将推出, Learn how to build and deploy an AI that can answer questions about local files. 建和部署可回答有关本地文件的问题的 AI。

Coming soon 即将推出, Learn how to build and deploy an AI chat bot that understands multiple knowledge bases. 了解如何构建和部署了解多个知识库的 AI 聊天机器人。

寻找更多创意？在GitHub上查看我们的[示例库]（https://platform.openai.com/examples）或[OpenAI Cookbook]（https://github.com/openai/openai-cookbook）。

## Terms & policies

https://openai.com/policies

 

## See Also

https://www.w3cschool.cn/openai_api/openai-api-introduction.html

http://chatgpt.cndoc.wiki/doc

































