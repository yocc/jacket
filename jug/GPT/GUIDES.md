# GUIDES 指引

[toc]



## Text completion

学习如何生成或者操作文本

### [Introduction](https://platform.openai.com/docs/guides/completion/introduction) 

[补全completions](https://platform.openai.com/docs/api-reference/completions)端点可用于各种任务。它为我们的任何[模型models](https://platform.openai.com/docs/models)提供了一个简单但功能强大的界面。您输入一些文本作为提示prompt，模型将生成一个文本补全completion，这个文本补全尝试匹配您为其提供的任何上下文或模式。例如，如果你给 API 提示prompt，“正如笛卡尔所说，我认为，因此”，它将以很高的概率probability返回补全completion“我是”。

input: prompt

output: text completion

开始探索补全completion的最佳方式是通过我们的 Playground。它只是一个文本框，您可以在其中提交提示prompt 以生成补全completion。要自己尝试一下，[在Playground中打开此示例]（https://platform.openai.com/playground/p/8P6JA6XEx74NTvcRUngWKEYW）：

> Write a tagline for an ice cream shop.

一旦你提交,你会看到这样的东西:

> Write a tagline for an ice cream shop. 
>
> **We serve up smiles with every scoop!**

您看到的实际actual补全completion情况可能有所不同，因为默认情况下 API 是不确定的。这意味着即使您的提示prompt保持不变，您每次调用它时可能会得到略微不同的补全completion。将 [temperature](https://platform.openai.com/docs/api-reference/completions/create#completions/create-temperature) 设置为 0 将使输出大部分具有确定性，但可能会保留少量可变性。

这个简单的文本输入、文本输出界面意味着您可以通过提供说明或您希望它做什么的几个示例来“编程”模型。它的成功通常取决于任务的复杂性和提示prompt的质量。一个好的经验法则是考虑如何为中学生写一个应用题来解决。一个写得很好的提示prompt提供了足够的信息让模型知道你想要什么以及它应该如何响应。

本指南涵盖了一般的提示设计最佳实践和示例。要了解有关使用我们的 Codex 模型处理代码的更多信息，请访问我们的[代码指南](https://platform.openai.com/docs/guides/code)。

请记住，默认模型的训练数据在 2021 年中断，因此它们可能不了解当前事件。我们计划在未来增加更多的持续培训。

### [Prompt design](https://platform.openai.com/docs/guides/completion/prompt-design) 提示设计

#### [Basics](https://platform.openai.com/docs/guides/completion/basics) 

我们的模型可以做任何事情，从生成原创故事到执行复杂的文本分析。因为他们可以做很多事情，所以你必须明确描述你想要什么。展示，而不仅仅是讲述，通常是获得良好提示prompt 的秘诀。

创建提示prompt 的三个基本准则：

**Show and tell 展示并讲述。**通过说明、示例或两者的结合，明确您想要什么。如果您希望模型按字母顺序对项目列表进行排名或按情绪对段落进行分类，请向它展示您想要的。

**Provide quality data 提供高质量的数据。**如果您正在尝试构建分类器或让模型遵循某种模式，请确保有足够的示例。一定要校对你的例子——这个模型通常足够聪明，可以看穿基本的拼写错误并给你一个回应，但它也可能认为这是故意的，它会影响回应。

**Check your settings 检查您的设置。**温度和 top_p 设置控制模型在生成响应时的确定性。如果您要求它提供只有一个正确答案的答复，那么您需要将这些设置得较低。如果您正在寻找更多样化的响应，那么您可能希望将它们设置得更高。人们在使用这些设置时犯的第一个错误是假设它们是“聪明”或“创造力”控制。

#### [Troubleshooting](https://platform.openai.com/docs/guides/completion/troubleshooting)

如果在使 API 按预期执行时遇到问题，请遵循以下清单：

1. 是否清楚预期的一代应该是什么？
2. 示例够吗？
3. 您是否检查过您的示例是否有错误？（API 不会直接告诉你）
4. 是否正确使用温度和top_p？

perform, 执行

#### [Classification](https://platform.openai.com/docs/guides/completion/classification)

为了使用 API 创建文本分类器，我们提供了任务的描述和一些示例。在此示例中，我们展示了如何对推文的情绪进行分类。

```
Decide whether a Tweet's sentiment is positive, neutral, or negative. 
Tweet: I loved the new Batman movie! 
Sentiment:

[Open in Playground](https://platform.openai.com/playground/p/default-tweet-classifier)
```

值得关注此示例中的几个功能：

1. **Use plain language to describe your inputs and outputs. 使用通俗易懂的语言来描述您的输入和输出。**我们使用通俗易懂的语言来描述输入“Tweet”和预期输出“Sentiment”。作为最佳实践，从简单的语言描述开始。虽然您通常可以使用速记或键来指示输入和输出，但最好从尽可能具有描述性开始，然后向后工作以删除多余的单词并查看性能是否保持一致。
2. **Show the API how to respond to any case. 向 API 显示如何响应任何情况。** 在本示例中，我们在指令中包含了可能的情绪标签。中性标签很重要，因为在很多情况下，即使是人类也很难确定某事是积极的还是消极的，以及两者都不是的情况。
3. **You need fewer examples for familiar tasks. 对于熟悉的任务，你需要更少的例子。**对于这个分类器，我们不提供任何例子。这是因为 API 已经理解了情绪和推文的概念。如果您正在为 API 可能不熟悉的内容构建分类器，则可能需要提供更多示例。

indicate, 指示

#### [Improving the classifier's efficiency](https://platform.openai.com/docs/guides/completion/improving-the-classifier-s-efficiency) 提高分类器的效率

现在我们已经掌握了如何构建分类器，让我们以这个例子为例，使其更加高效，以便我们可以使用它从一个 API 调用中获取多个结果。

```
Classify the sentiment in these tweets: 
1. "I can't stand homework" 
2. "This sucks. I'm bored 😠" 
3. "I can't wait for Halloween!!!" 
4. "My cat is adorable ❤️❤️" 
5. "I hate chocolate" Tweet sentiment ratings:

[Open in Playground](https://platform.openai.com/playground/p/default-adv-tweet-classifier)
```

classifier, 分类器

我们提供了一个带编号的推文列表，因此 API 可以在一次 API 调用中对五条（甚至更多）推文进行评级。

请务必注意，当您要求 API 创建列表或评估文本时，您需要特别注意概率设置（Top P 或温度）以避免漂移。

1. 通过运行多个测试，确保您的概率设置正确校准。
2. 不要让你的列表太长，否则 API 可能会漂移。

#### [Generation](https://platform.openai.com/docs/guides/completion/generation) 生成

One of the most powerful yet simplest tasks you can accomplish with the API is generating new ideas or versions of input. You can ask for anything from story ideas, to business plans, to character descriptions and marketing slogans. In this example, we'll use the API to create ideas for using virtual reality in fitness.

使用 API 可以完成的最强大但最简单的任务之一是生成新的想法或输入版本。你可以要求任何东西，从故事创意到商业计划，到角色描述和营销口号。在此示例中，我们将使用 API 创建在健身中使用虚拟现实的想法。

```
Brainstorm some ideas combining VR and fitness:

[Open in Playground](https://platform.openai.com/playground/p/default-vr-fitness)
```

If needed, you can improve the quality of the responses by including some examples in your prompt.

如果需要，您可以通过在提示中包含一些示例来提高响应的质量。

#### [Conversation](https://platform.openai.com/docs/guides/completion/conversation) 对话

The API is extremely adept at carrying on conversations with humans and even with itself. With just a few lines of instruction, we've seen the API perform as a customer service chatbot that intelligently answers questions without ever getting flustered or a wise-cracking conversation partner that makes jokes and puns. The key is to tell the API how it should behave and then provide a few examples.

Here's an example of the API playing the role of an AI answering questions:

该 API 非常擅长与人类甚至自身进行对话。只需几行指令，我们就看到 API 可以作为客户服务聊天机器人执行，它可以智能地回答问题而不会感到慌乱，或者是一个明智的对话伙伴，可以开玩笑和双关语。关键是告诉 API 它应该如何运行，然后提供一些示例。

下面是 API 扮演 AI 回答问题的角色的示例：

```
The following is a conversation with an AI assistant. The assistant is helpful, creative, clever, and very friendly. 

Human: Hello, who are you? 
AI: I am an AI created by OpenAI. How can I help you today? 
Human:

[Open in Playground](https://platform.openai.com/playground/p/default-chat)
```

This is all it takes to create a chatbot capable of carrying on a conversation. Underneath its simplicity, there are several things going on that are worth paying attention to:

1. **We tell the API the intent but we also tell it how to behave.** Just like the other prompts, we cue the API into what the example represents, but we also add another key detail: we give it explicit instructions on how to interact with the phrase "The assistant is helpful, creative, clever, and very friendly."

   Without that instruction the API might stray and mimic the human it's interacting with and become sarcastic or some other behavior we want to avoid.

2. **We give the API an identity.** At the start we have the API respond as an AI assistant. While the API has no intrinsic identity, this helps it respond in a way that's as close to the truth as possible. You can use identity in other ways to create other kinds of chatbots. If you tell the API to respond as a woman who works as a research scientist in biology, you'll get intelligent and thoughtful comments from the API similar to what you'd expect from someone with that background.

In this example we create a chatbot that is a bit sarcastic and reluctantly answers questions:

这就是创建一个能够进行对话的聊天机器人所需要的一切。在它的简单之下，有几件事值得关注：

1. **我们告诉 API 意图，但我们也告诉它如何行为。 就像其他提示一样，我们将 API 提示到示例所代表的内容中，但我们还添加了另一个关键细节：我们向它提供了有关如何与短语“助手乐于助人、富有创造力、聪明且非常友好”进行交互的明确说明。

   如果没有该指令，API 可能会偏离并模仿与之交互的人类，并变得讽刺或我们想要避免的其他行为。

2. **我们给 API 一个身份。 一开始，我们让 API 作为 AI 助手进行响应。虽然 API 没有固有标识，但这有助于它以尽可能接近事实的方式进行响应。您可以通过其他方式使用身份来创建其他类型的聊天机器人。如果你告诉 API 以生物学研究科学家的身份做出回应，你会从 API 得到聪明而深思熟虑的评论，类似于你对具有这种背景的人的期望。

在这个例子中，我们创建了一个有点讽刺的聊天机器人，它不情愿地回答问题：

```
Marv is a chatbot that reluctantly answers questions with sarcastic responses: 

You: How many pounds are in a kilogram? 
Marv: This again? There are 2.2 pounds in a kilogram. Please make a note of this. 
You: What does HTML stand for? 
Marv: Was Google too busy? Hypertext Markup Language. The T is for try to ask better questions in the future. 
You: When did the first airplane fly? 
Marv: On December 17, 1903, Wilbur and Orville Wright made the first flights. I wish they’d come and take me away. 
You: What is the meaning of life? 
Marv: I’m not sure. I’ll ask my friend Google. 
You: Why is the sky blue?

[Open in Playground](https://platform.openai.com/playground/p/default-marv-sarcastic-chat)
```

To create an amusing and somewhat helpful chatbot, we provide a few examples of questions and answers showing the API how to reply. All it takes is just a few sarcastic responses, and the API is able to pick up the pattern and provide an endless number of snarky responses.

为了创建一个有趣且有点有用的聊天机器人，我们提供了一些问题和答案示例，展示了 API 如何回复。只需要几个讽刺的响应，API 就能够拾取模式并提供无穷无尽的尖刻响应。

#### [Transformation](https://platform.openai.com/docs/guides/completion/transformation) 转换

The API is a language model that is familiar with a variety of ways that words and characters can be used to express information. This ranges from natural language text to code and languages other than English. The API is also able to understand content on a level that allows it to summarize, convert and express it in different ways.

API 是一种语言模型，它熟悉单词和字符可用于表达信息的各种方式。范围从自然语言文本到代码和英语以外的语言。API 还能够在允许它以不同方式总结、转换和表达内容的级别上理解内容。

#### [Translation](https://platform.openai.com/docs/guides/completion/translation) 翻译

In this example we show the API how to convert from English to French, Spanish, and Japanese:

在此示例中，我们向 API 展示了如何将英语转换为法语、西班牙语和日语：

```
Translate this into French, Spanish and Japanese: What rooms do you have available?

[Open in Playground](https://platform.openai.com/playground/p/default-translate)
```

This example works because the API already has a grasp of these languages, so there's no need to try to teach them.

If you want to translate from English to a language the API is unfamiliar with, you'd need to provide it with more examples or even [fine-tune a model](https://platform.openai.com/docs/guides/fine-tuning) to do it fluently.

此示例之所以有效，是因为 API 已经掌握了这些语言，因此无需尝试教授它们。

如果你想从英语翻译成API不熟悉的语言，你需要为它提供更多的例子，甚至[微调模型]（https://platform.openai.com/docs/guides/fine 调优）才能流利地做到这一点。

#### [Conversion](https://platform.openai.com/docs/guides/completion/conversion) 变换

In this example we convert the name of a movie into emoji. This shows the adaptability of the API to picking up patterns and working with other characters.

在此示例中，我们将电影名称转换为表情符号。这显示了 API 对拾取模式和处理其他字符的适应性。

```
Convert movie titles into emoji. Back to the Future: 👨👴🚗🕒  Batman: 🤵🦇  Transformers: 🚗🤖  Star Wars:

[Open in Playground](https://platform.openai.com/playground/p/default-movie-to-emoji)
```

#### [Summarization](https://platform.openai.com/docs/guides/completion/summarization)

The API is able to grasp the context of text and rephrase it in different ways. In this example, we create an explanation a child would understand from a longer, more sophisticated text passage. This illustrates that the API has a deep grasp of language.

该▁API▁能够掌握文本的上下文,并以不同的方式对其进行重述。在这个示例中,我们创建了一个孩子可以从更长、更复杂的文本段落中理解的解释。这表明▁API▁对语言有着深刻的理解。

```
Summarize this for a second-grade student: 

Jupiter is the fifth planet from the Sun and the largest in the Solar System. It is a gas giant with a mass one-thousandth that of the Sun, but two-and-a-half times that of all the other planets in the Solar System combined. Jupiter is one of the brightest objects visible to the naked eye in the night sky, and has been known to ancient civilizations since before recorded history. It is named after the Roman god Jupiter.[19] When viewed from Earth, Jupiter can be bright enough for its reflected light to cast visible shadows,[20] and is on average the third-brightest natural object in the night sky after the Moon and Venus.

[Open in Playground](https://platform.openai.com/playground/p/default-summarize)
```

#### [Completion](https://platform.openai.com/docs/guides/completion/prompt-design-completion)

While all prompts result in completions, it can be helpful to think of text completion as its own task in instances where you want the API to pick up where you left off. For example, if given this prompt, the API will continue the train of thought about vertical farming. You can lower the [temperature](https://platform.openai.com/docs/api-reference/completions/create#completions/create-temperature) setting to keep the API more focused on the intent of the prompt or increase it to let it go off on a tangent.

虽然所有提示都会导致完成，但在您希望 API 从上次中断的地方继续的情况下，将文本完成视为自己的任务可能会很有帮助。例如，如果给出此提示，API 将继续思考垂直农业。您可以降低 [温度]（https://platform.openai.com/docs/api-引用/完成/创建#完成/创建温度）设置，以使 API 更专注于提示的意图，或者增加它以使其偏离正切线。

```
Vertical farming provides a novel solution for producing food locally, reducing transportation costs and

[Open in Playground](https://platform.openai.com/playground/p/XCHYTQToJ31wbcxJykv2as8u)
```

This next prompt shows how you can use completion to help write React components. We send some code to the API, and it's able to continue the rest because it has an understanding of the React library. We recommend using our [Codex models](https://platform.openai.com/docs/models/codex) for tasks that involve understanding or generating code. To learn more, visit our [code guide](https://platform.openai.com/docs/guides/code).

下一个提示显示了如何使用完成来帮助编写 React 组件。我们向 API 发送一些代码，它能够继续其余的代码，因为它了解 React 库。我们建议将我们的 [Codex 模型]（https://platform.openai.com/docs/models/codex） 用于涉及理解或生成代码的任务。要了解更多信息，请访问我们的 [代码指南]（https://platform.openai.com/docs/guides/code）。

```
import React from 'react'; 
const HeaderComponent = () => (

[Open in Playground](https://platform.openai.com/playground/p/12d6OQCSrFppNeMHOQjtyk8T)
```

#### [Factual responses](https://platform.openai.com/docs/guides/completion/factual-responses) 事实答复

The API has a lot of knowledge that it's learned from the data that it was been trained on. It also has the ability to provide responses that sound very real but are in fact made up. There are two ways to limit the likelihood of the API making up an answer.

1. **Provide a ground truth for the API.** If you provide the API with a body of text to answer questions about (like a Wikipedia entry) it will be less likely to confabulate a response.
2. **Use a low probability and show the API how to say "I don't know".** If the API understands that in cases where it's less certain about a response that saying "I don't know" or some variation is appropriate, it will be less inclined to make up answers.

In this example we give the API examples of questions and answers it knows and then examples of things it wouldn't know and provide question marks. We also set the probability to zero so the API is more likely to respond with a "?" if there is any doubt.

API 有很多知识，它是从训练它的数据中学习的。它还能够提供听起来非常真实但实际上是编造的响应。有两种方法可以限制 API 组成答案的可能性。

1. **为 API 提供基本事实。 如果您向 API 提供文本正文来回答有关（如维基百科条目）的问题，则不太可能虚构响应。
2. **使用低概率并向 API 展示如何说“我不知道”.** 如果 API 明白，在不太确定回答的情况下，说“我不知道”或某些变体是合适的，它将不太倾向于编造答案。

在此示例中，我们向 API 提供它知道的问题和答案的示例，然后提供它不知道的事情的示例并提供问号。我们还将概率设置为零，以便 API 更有可能在有任何疑问时响应“？”。

```
Q: Who is Batman? 
A: Batman is a fictional comic book character. 

Q: What is torsalplexity? 
A: ? 

Q: What is Devz9? 
A: ? 

Q: Who is George Lucas? 
A: George Lucas is American film director and producer famous for creating Star Wars. 

Q: What is the capital of California? 
A: Sacramento. 

Q: What orbits the Earth? 
A: The Moon. 

Q: Who is Fred Rickerson? 
A: ? 

Q: What is an atom? 
A: An atom is a tiny particle that makes up everything. 

Q: Who is Alvan Muntz? 
A: ? 

Q: What is Kozar-09? 
A: ? 

Q: How many moons does Mars have? 
A: Two, Phobos and Deimos. 

Q:

[Open in Playground](https://platform.openai.com/playground/p/HMoho4552EHXrPLbmOIxpX4X)
```

### Inserting text Beta 插入文本

The completions endpoint also supports inserting text within text by providing a [suffix prompt](https://platform.openai.com/docs/api-reference/completions/create#completions/create-suffix) in addition to the [prefix prompt](https://platform.openai.com/docs/api-reference/completions/create#completions/create-prompt). This need naturally arises when writing long-form text, transitioning between paragraphs, following an outline, or guiding the model towards an ending. This also works on code, and can be used to insert in the middle of a function or file. Visit our [code guide](https://platform.openai.com/docs/guides/code/inserting-text) to learn more.

To illustrate how important suffix context is to our ability to predict, consider the prompt, “Today I decided to make a big change.” There’s many ways one could imagine completing the sentence. But if we now supply the ending of the story: “I’ve gotten many compliments on my new hair!”, the intended completion becomes clear.

完成终结点还支持在文本中插入文本，方法是提供 [后缀提示]（https://platform.openai.com/docs/api-引用/完成/创建#完成/创建后缀）以及 [前缀提示]（https://platform.openai.com/docs/api-引用/完成/创建#完成/创建-提示）。在编写长篇文本、段落之间过渡、遵循大纲或引导模型走向结尾时，自然会出现这种需求。这也适用于代码，可用于插入函数或文件的中间。访问我们的 [代码指南]（https://platform.openai.com/docs/guides/code/inserting-文本）以了解更多信息。

为了说明后缀上下文对我们的预测能力有多重要，请考虑提示，“今天我决定做出重大改变。人们可以通过多种方式想象完成句子。但是，如果我们现在提供故事的结局：“我的新头发得到了很多赞美！”，预期的完成就变得清晰了。

> I went to college at Boston University. After getting my degree, I decided to make a change**. A big change!**  
>
> **I packed my bags and moved to the west coast of the United States.** 
>
>  Now, I can’t get enough of the Pacific Ocean!

By providing the model with additional context, it can be much more steerable. However, this is a more constrained and challenging task for the model.

通过为模型提供额外的上下文，它可以更具可操纵性。但是，对于模型来说，这是一项更具限制和挑战性的任务。

#### [Best practices](https://platform.openai.com/docs/guides/completion/best-practices)

Inserting text is a new feature in beta and you may have to modify the way you use the API for better results. Here are a few best practices:

**Use max_tokens > 256.** The model is better at inserting longer completions. With too small max_tokens, the model may be cut off before it's able to connect to the suffix. Note that you will only be charged for the number of tokens produced even when using larger max_tokens.

**Prefer finish_reason == "stop".** When the model reaches a natural stopping point or a user provided stop sequence, it will set finish_reason as "stop". This indicates that the model has managed to connect to the suffix well and is a good signal for the quality of a completion. This is especially relevant for choosing between a few completions when using n > 1 or resampling (see the next point).

**Resample 3-5 times.** While almost all completions connect to the prefix, the model may struggle to connect the suffix in harder cases. We find that resampling 3 or 5 times (or using best_of with k=3,5) and picking the samples with "stop" as their finish_reason can be an effective way in such cases. While resampling, you would typically want a higher temperatures to increase diversity.

Note: if all the returned samples have finish_reason == "length", it's likely that max_tokens is too small and model runs out of tokens before it manages to connect the prompt and the suffix naturally. Consider increasing max_tokens before resampling.

**Try giving more clues.** In some cases to better help the model’s generation, you can provide clues by giving a few examples of patterns that the model can follow to decide a natural place to stop.

插入文本是 Beta 版的一项新功能，您可能需要修改 API 的使用方式以获得更好的结果。以下是一些最佳实践：

**使用 max_tokens > 256。** 该模型更擅长插入更长的补全。如果 max_tokens 太小，模型可能会在连接到后缀之前被切断。请注意，即使使用更大的 max_tokens，您也只会根据生成的代币数量付费。

**首选 finish_reason == "stop"。** 当模型到达自然停止点或用户提供的停止序列时，它将 finish_reason 设置为 "stop"。这表明该模型已设法很好地连接到后缀，并且是完成质量的良好信号。这对于在使用 n > 1 或重采样时在几个完成之间进行选择尤其相关（请参阅下一点）。

**重采样 3-5 次。** 虽然几乎所有补全都连接到前缀，但在更困难的情况下，模型可能难以连接后缀。我们发现，在这种情况下，重采样 3 次或 5 次（或使用 k=3,5 的 best_of）并选择带有“stop”作为其 finish_reason 的样本可能是一种有效的方法。重采样时，您通常需要更高的温度来增加多样性。

注意：如果所有返回的样本都有 finish_reason == "length"，很可能是 max_tokens 太小，模型在设法自然地连接提示和后缀之前用完了标记。考虑在重采样之前增加 max_tokens。

**尝试提供更多线索。**在某些情况下，为了更好地帮助模型的生成，您可以通过提供一些模式示例来提供线索，模型可以遵循这些模式来决定自然停止的位置。

> How to make a delicious hot chocolate: 
>
> 1. **Boil water**
> 2.  **2. Put hot chocolate in a cup**
> 3.  **3. Add boiling water to the cup**
> 4.  4. Enjoy the hot chocolate

> 1. Dogs are loyal animals. 
> 1. Lions are ferocious animals. 
> 1. Dolphins **are playful animals.**
> 1. Horses are majestic animals.

### Editing text Alpha 编辑文本 Alpha

The [edits](https://platform.openai.com/docs/api-reference/edits) endpoint can be used to edit text, rather than just completing it. You provide some text and an instruction for how to modify it, and the `text-davinci-edit-001` model will attempt to edit it accordingly. This is a natural interface for translating, editing, and tweaking text. This is also useful for refactoring and working with code. Visit our [code guide](https://platform.openai.com/docs/guides/code/editing-text) to learn more. During this initial beta period, usage of the edits endpoint is free.

[edits](https://platform.openai.com/docs/api-reference/edits) 端点可用于编辑文本，而不仅仅是完成文本。您提供一些文本和如何修改它的说明，“text-davinci-edit-001”模型将尝试相应地对其进行编辑。这是用于翻译、编辑和调整文本的自然界面。这对于重构和使用代码也很有用。访问我们的[代码指南](https://platform.openai.com/docs/guides/code/editing-text) 了解更多信息。在此初始测试期间，编辑端点的使用是免费的。

[Examples](https://platform.openai.com/docs/guides/completion/examples)

INPUT

> GPT-3 is a very nice AI 
>
> That's pretty good at writing replies 
>
> When it's asked a question 
>
> It gives its suggestion 
>
> This is a poem it made that rhymes

INSTRUCTIONS

> Make this in the voice of GPT-3

OUTPUT

> **I am** a very nice AI
>
> **I am** pretty good at writing replies
>
> When **I am** asked a question
>
> **I give my** suggestion
>
> This is a poem **I** made that rhymes















## See Also

https://www.w3cschool.cn/openai_api/openai-api-introduction.html

http://chatgpt.cndoc.wiki/doc

https://platform.openai.com/docs/guides/completion