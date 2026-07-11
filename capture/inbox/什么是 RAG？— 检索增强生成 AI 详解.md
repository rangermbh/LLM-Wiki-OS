---
created: 2026-07-11T23:48:04+08:00
source: https://aws.amazon.com/what-is/retrieval-augmented-generation/
source_type: web-clip
tags:
  - clippings
status: processed
processed_date: 2026-07-11
ingested_to:
  - "[[spaces/ai/wiki/concepts/rag]]"
agent_version: claude-opus-4.7
---

# 什么是 RAG？— 检索增强生成 AI 详解

## Source Content

## 什么是 RAG（检索增强生成）？

## 什么是检索增强生成？

检索增强生成（RAG）是指对大语言模型输出进行优化，使其能够在生成响应之前引用训练数据来源之外的权威知识库。大语言模型（LLM）用海量数据进行训练，使用数十亿个参数为回答问题、翻译语言和完成句子等任务生成原始输出。在 LLM 本就强大的功能基础上，RAG 将其扩展为能访问特定领域或组织的内部知识库，所有这些都无需重新训练模型。这是一种经济高效地改进 LLM 输出的方法，让它在各种情境下都能保持相关性、准确性和实用性。

## 为什么检索增强生成很重要？

LLM 是一项关键的 [人工智能（AI）](https://aws.amazon.com/what-is/artificial-intelligence/) 技术，为智能 [聊天机器人](https://aws.amazon.com/what-is/chatbot/) 和其他 [自然语言处理（NLP）](https://aws.amazon.com/what-is/nlp/) 应用程序提供支持。目标创建能够是通过交叉引用权威知识来源，在各种环境中回答用户问题的机器人。不幸的是，LLM 技术的性质给 LLM 响应带来了不可预测性。此外，LLM 训练数据是静态的，从而为其掌握的知识限定了截止日期。

LLM 面临的已知挑战包括：

- 在没有答案的情况下提供虚假信息。
- 在用户需要具体的最新响应时，提供过时或宽泛的信息。
- 依据非权威来源创建响应。
- 由于术语混淆，不同的培训来源使用相同的术语来谈论不同的事情，因此会产生不准确的响应。

您可以将 [大语言模型](https://aws.amazon.com/what-is/large-language-model/) 看作是一个过于热情的新员工，他拒绝随时了解时事，但总是会绝对自信地回答每一个问题。不幸的是，这种态度会对用户的信任产生负面影响，这是您不希望聊天机器人效仿的！

RAG 是解决其中一些挑战的一种方法。它会重定向 LLM，从权威的、预先确定的知识来源中检索相关信息。组织可以更好地控制生成的文本输出，并且用户可以深入了解 LLM 如何生成响应。

## 检索增强生成有哪些好处？

RAG 技术为组织的 [生成式人工智能](https://aws.amazon.com/what-is/generative-ai/) 工作带来了多项好处。

### 经济高效的实施

聊天机器人开发通常从 [基础模型](https://aws.amazon.com/what-is/foundation-models/) 开始。基础模型（FM）是在广泛的广义和未标记数据上训练的 API 可访问 LLM。针对组织或领域特定信息重新训练基础模型的计算和财务成本很高。RAG 是一种将新数据引入 LLM 的更加经济高效的方法。它使生成式人工智能技术更广泛地得以获取和使用。

### 当前信息

即使 LLM 的原始训练数据来源适合您的需求，但保持相关性也具有挑战性。RAG 允许开发人员为生成模型提供最新的研究、统计数据或新闻。他们可以使用 RAG 将 LLM 直接连接到实时社交媒体提要、新闻网站或其他经常更新的信息来源。LLM 随即可以向用户提供最新信息。

### 增强用户信任度

RAG 允许 LLM 通过来源归属来呈现准确的信息。输出可以包括对来源的引文或引用。如果需要进一步说明或更详细的信息，用户也可以自己查找源文档。这可以增加对您的生成式人工智能解决方案的信任和信心。

### 更多开发人员控制权

借助 RAG，开发人员可以更高效地测试和改进他们的聊天应用程序。他们可以控制和更改 LLM 的信息来源，以适应不断变化的需求或跨职能使用。开发人员还可以将敏感信息的检索限制在不同的授权级别内，并确保 LLM 生成适当的响应。此外，如果 LLM 针对特定问题引用了错误的信息来源，他们还可以进行故障排除并进行修复。组织可以更自信地为更广泛的应用程序实施生成式人工智能技术。

## 检索增强生成的工作原理是什么？

如果没有 RAG，LLM 会接收用户输入，并根据训练信息（即其已知信息）创建响应。如果有 RAG，则会引入一个信息检索组件，利用用户输入首先从新数据源提取信息。用户查询和相关信息都提供给 LLM。LLM 使用新知识及其训练数据来创建更好的响应。以下各部分概述了该过程。

### 创建外部数据

LLM 原始训练数据集之外的新数据称为 *外部数据* 。它可以来自多个数据来源，例如 API、数据库或文档存储库。数据可能以各种格式存在，例如文件、数据库记录或长篇文本。另一种称为 *嵌入语言模型* 的人工智能技术将数据转换为数字表示形式并将其存储在向量数据库中。这个过程会创建一个生成式人工智能模型可以理解的知识库。

[了解 Amazon OpenSearch 向量搜索](https://aws.amazon.com/cn/opensearch-service/)

### 检索相关信息

下一步是执行相关性搜索。用户查询将转换为向量表示形式，并与向量数据库匹配。例如，考虑一个可以回答组织的人力资源问题的智能聊天机器人。如果员工搜索 *：“我有多少年假？”* ，系统将检索年假政策文档以及员工个人过去的休假记录。将会返回这些与员工输入的内容高度相关的特定文档。相关性是使用数学向量计算和表示法计算得出并确立的。

### 增强 LLM 提示

接下来，RAG 模型通过在上下文中添加检索到的相关数据来增强用户输入（或提示）。此步骤使用提示工程技术与 LLM 进行有效沟通。增强提示允许大语言模型为用户查询生成准确的答案。

### 更新外部数据

下一个问题可能是——如果外部数据过时了怎么办？ 要维护当前信息以供检索，请异步更新文档并更新文档的嵌入表示形式。您可以通过自动化实时流程或定期批处理来执行此操作。这是数据分析中常见的挑战——可以使用不同的数据科学方法进行变更管理。

下图显示了将 RAG 与 LLM 配合使用的概念流程。

![](https://docs.aws.amazon.com/images/sagemaker/latest/dg/images/jumpstart/jumpstart-fm-rag.jpg)

## 检索增强生成和语义搜索有什么区别？

语义搜索可以完善 RAG 结果，适用于想要在其 LLM 应用程序中添加大量外部知识源的组织。现代企业在各种系统中存储大量信息，例如手册、常见问题、研究报告、客户服务指南和人力资源文档存储库等。上下文检索在规模上具有挑战性，因此会降低生成输出质量。

语义搜索技术可以扫描包含不同信息的大型数据库，并更准确地检索数据。例如，他们可以回答诸如 *“去年在机械维修上花了多少钱？”* 之类的问题，方法是将问题映射到相关文档并返回特定文本而不是搜索结果。然后，开发人员可以使用该答案为 LLM 提供更多上下文。

RAG 中的传统或关键字搜索解决方案对知识密集型任务产生的结果有限。开发人员在手动准备数据时还必须处理单词嵌入、文档分块和其他复杂问题。相比之下，语义搜索技术可以完成知识库准备的所有工作，因此开发人员不必这样做。它们还生成语义相关的段落和按相关性排序的标记词，以最大限度地提高 RAG 有效载荷的质量。

## AWS 如何支持您的检索增强生成需求？

[Amazon Bedrock](https://aws.amazon.com/bedrock/) 是一项完全托管的服务，提供多种高性能基础模型以及多种功能，用于构建生成式人工智能应用程序，同时简化开发并维护隐私和安全。借助 Amazon Bedrock 的知识库，您只需点击几下即可将基础模型连接到您的 RAG 数据来源。向量转换、检索和改进的输出生成均自动处理。

对于管理自己的 RAG 的组织来说， [Amazon Kendra](https://aws.amazon.com/kendra/) 是一项由机器学习提供支持的高精度企业搜索服务。它提供了经过优化的 Kendra [检索 API](https://docs.aws.amazon.com/kendra/latest/APIReference/API_Retrieve.html) ，您可以将其与 Amazon Kendra 的高精度语义排名器一起使用，作为 RAG 工作流程的企业检索器。例如，使用检索 API，您可以：

- 检索多达 100 个语义相关的段落，每个段落最多包含 200 个标记词，按相关性排序。
- 使用预构建的连接器连接到常用数据技术，例如 [Amazon Simple Storage Service](https://aws.amazon.com/s3/) 、SharePoint、Confluence 和其他网站。
- 支持多种文档格式，例如 HTML、Word、PowerPoint、PDF、Excel 和文本文件。
- 根据最终用户权限允许的文档筛选响应。

亚马逊还为想要构建更多自定义生成式人工智能解决方案的组织提供了选项。 [Amazon SageMaker JumpStart](https://aws.amazon.com/sagemaker/jumpstart/) 是一个机器学习中心，包含基础模型、内置算法和预构建的机器学习解决方案，只需点击几下即可轻松部署。您可以通过参考现有的 SageMaker 笔记本和代码示例来加快 RAG 的实施。

[立即创建免费账户](https://portal.aws.amazon.com/billing/signup) ，开始在 AWS 上使用检索增强生成

[使用 Bedrock Knowledge Bases 构建 RAG 应用](https://aws.amazon.com/cn/bedrock/knowledge-bases/)

## AWS 上的后续步骤

### [查看其他与产品相关的资源](https://aws.amazon.com/ai/generative-ai/services/)

使用极为全面的生成式人工智能服务组合加速创新

### [注册免费账户](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html)

立即享受 AWS Free Tier。

注册

### [开始在控制台中构建](https://console.aws.amazon.com/)

在 AWS 管理控制台中开始构建。

登录

## Browse all cloud computing concepts

Browse all cloud computing concepts content here:[Compute

#### 什么是.Net？

.NET 是一个开源平台，用于构建能够在任何操作系统上本地运行的桌面、Web 和移动应用程序。.NET 系统包括支持现代、可扩展和高性能软件开发的工具、库和语言。.NET 平台有一个活跃的开发人员社区来维护和支持。

简单地说，.NET 平台软件可以完成以下任务：

- 将.NET 编程语言代码转换为计算设备可以处理的指令。
- 为高效的软件开发提供实用工具。例如，这种工具可以查找当前时间或在屏幕上打印文本。
- 定义一组数据类型，以在计算机上存储文本、数字和日期等信息。

了解更多

](https://aws.amazon.com/what-is/net/?trk=faq_card)#### 什么是运营分析？

Data & Analytics

2022-10-25

运营分析是一种解决方案，可使用多个不同的数据日志让您了解系统的运行状况。其工作原理类似于向您的主治医生、牙医和心脏病科医师问诊，以全面了解您的健康状况。所有这些不同的系统（医生）将它们的日志（检查结果）组合在一起，以显示问题所在以及解决问题的最佳方式。运营分析是以相同的理念驱动的。

了解更多

[View original](https://aws.amazon.com/what-is/operational-analytics/?trk=faq_card)[Generative Design, and Generative AI in Engineering and Beyond (featuring Steve Hooper, VP for design and manufacturing software tools at Autodesk)

2023-12-07

#### 什么是 LangChain？

LangChain 是一个开源框架，用于构建基于大型语言模型（LLM）的应用程序。LLM 是基于大量数据预先训练的大型深度学习模型，可以生成对用户查询的响应，例如回答问题或根据基于文本的提示创建图像。LangChain 提供各种工具和抽象，以提高模型生成的信息的定制性、准确性和相关性。例如，开发人员可以使用 LangChain 组件来构建新的提示链或自定义现有模板。LangChain 还包括一些组件，可让 LLM 无需重新训练即可访问新的数据集。

了解更多

](https://aws.amazon.com/what-is/langchain/?trk=faq_card)#### 什么是人工智能（AI）？

Artificial Intelligence vs Machine Learning

2023-09-20

人工智能（AI）是致力于解决通常与人类智能相关联的认知性问题的计算机科学领域，这些问题包括学习、创造和图像识别等。现代组织从各种来源收集大量数据，例如智能传感器、人工生成的内容、监控工具和系统日志。人工智能的目标是创建从数据中获取意义的自我学习系统。然后，人工智能可以应用这些知识以类似人类的方式解决新问题。例如，人工智能技术可以对人类对话做出有意义的响应，创建原始图像和文本，并根据实时数据输入做出决策。您的组织可以在您的应用程序中集成 AI 功能，以优化业务流程、改善客户体验并加快创新。

了解更多

[View original](https://aws.amazon.com/what-is/artificial-intelligence/?trk=faq_card)#### 什么是流数据？

Data & Analytics

2023-11-06

流数据是以连续、增量方式大量发送的数据，目标是实现低延迟处理。组织有数千个数据来源，这些数据源通常同时发出消息、记录或数据，大小从几字节到几兆字节不等。它包括位置、事件和传感器数据，公司使用这些数据进行实时分析，以了解其业务的许多方面情况。例如，公司可以通过持续分析社交媒体流中的点击流和客户帖子，然后根据需要及时做出响应，来跟踪公众对其品牌和产品的情感变化。

了解更多

[View original](https://aws.amazon.com/what-is/streaming-data/?trk=faq_card)#### 什么是持续集成？

Developer Tools

2021-09-29

持续集成是一种 DevOps 软件开发实践。采用持续集成时，开发人员会定期将代码变更合并到一个中央存储库中，之后系统会自动运行构建和测试操作。持续集成通常涉及软件发布流程的构建或集成阶段，需要包含自动化（例如 CI 或构建服务）和知识（例如学习频繁集成）方面的内容。持续集成的主要目标是更快发现并解决错误，提高软件质量，并缩短验证和发布新软件更新所需的时间。

了解更多

[View original](https://aws.amazon.com/devops/continuous-integration/?trk=faq_card)

## Did you find what you were looking for today?

Let us know so we can improve the quality of the content on our pages

## Initial Notes
