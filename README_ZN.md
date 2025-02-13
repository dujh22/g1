
# **g1: 在 Groq 上使用 Llama-3.1 70b 创建类似 o1 的推理链**

[视频演示](https://github.com/user-attachments/assets/db2a221f-f8eb-48c3-b5a7-8399c6300243)

这是一个早期原型，利用提示策略通过类似 o1 的推理链来提升 LLM 的推理能力。这使得 LLM 能够“思考”并解决通常难倒主流模型的逻辑问题。与 o1 不同的是，所有的推理步骤都是可见的，并且该应用使用开源模型。

g1 是实验性质的，开源的目的是激励开源社区开发新的策略来实现类似 o1 的推理链。该实验展示了通过可视化步骤进行推理的潜力，而不是与 o1 进行比较或完全复制 o1。o1 使用大规模强化学习训练，通过链式思考（Chain of Thought）实现复杂博士水平问题的最先进表现。

g1 展示了仅靠提示如何克服 LLM 逻辑问题，例如草莓问题（Strawberry problem），让现有的开源模型受益于动态推理链和改进的探索界面。

**工作原理**

g1 由 Llama-3.1-70b 驱动，创建了动态链式思考的推理链，使 LLM 能够“思考”并解决通常难倒主流模型的一些逻辑问题。

在每个步骤中，LLM 可以选择继续下一个推理步骤，或者提供最终答案。每个步骤都有标题并对用户可见。系统提示还包括对 LLM 的提示建议。提示细分中有完整说明，一些示例包括要求模型“探索替代答案”，以及“使用至少三种方法推导答案”。

通过结合链式思考、尝试多种方法、探索替代答案、质疑草稿解决方案和考虑 LLM 的局限性，这种推理能力得到了提高。仅通过提示（无需训练），可以在草莓问题上达到约 70% 的准确率（n=10，“strawberry 中有几个 R？”）。未提示时，Llama-3.1-70b 准确率为 0%，ChatGPT-4o 为 30%。

**示例**

**	**[!重要]

g1 并不完美，但在简单逻辑问题上的表现明显优于开箱即用的 LLM。初步测试表明，g1 对通常难倒 LLM 的简单逻辑问题的解决准确率为 60%-80%。不过，准确率尚未正式评估。以下是一些示例。

**strawberry 中有几个 R？**

提示：strawberry 中有几个 R？

结果：

提示：哪个更大，.9 还是 .11？

结果：

**快速开始**

要使用 Streamlit UI，请按照以下说明操作：

python3 -m venv venv

source venv/bin/activate

pip3 install -r requirements.txt

export GROQ_API_KEY=gsk...

streamlit run app.py

或者，按照以下附加说明使用 Gradio UI：

cd gradio

pip3 install -r requirements.txt

python3 app.py

**提示策略**

提示如下：

你是一名专家级 AI 助手，会逐步解释你的推理过程。对于每个步骤，提供一个描述你在该步骤所做工作的标题和内容。决定是否需要另一个步骤，或者可以给出最终答案。以 JSON 格式响应，包含 'title'、'content' 和 'next_action'（可以是 'continue' 或 'final_answer'）键。使用尽可能多的推理步骤，至少 **3** 步。注意你作为 LLM 的局限性，以及你可以和不能做的事情。在推理中，探索替代答案。考虑你可能是错误的，并指出如果你的推理出错，可能会出现在什么地方。全面测试所有其他可能性。你可能是错误的。当你说你在重新检查时，实际上要重新检查，并使用另一种方法进行检查，而不仅仅是说你在重新检查。使用至少三种方法推导答案。使用最佳实践。

**细分**

**	**1.**	**添加身份：你是一名专家级 AI 助手，会逐步解释你的推理过程。

**	**2.**	**指导推理过程：对于每个步骤，提供标题和内容，决定是否继续或结束。

**	**3.**	**提供 JSON 格式规范和示例。

**	**4.**	**通过全大写字母强调重要提示，确保模型遵守：使用至少 3 步推理，探索替代答案，重新检查问题等。

**	**示例响应：

{

**    **"title"**: **"识别关键信息"**,**

**    **"content"**: **"为了开始解决这个问题，我们需要仔细检查所给的信息，并识别出指导解决过程的关键要素。这包括..."**,**

**    **"next_action"**: **"continue"

}

**相关项目**

**	**•**	**Huggingface Spaces Demo：[Hugging Face Spaces](https://huggingface.co/spaces/xylin/g1-demo)

**	**•**	**Mult1：使用多 AI 提供商创建类似 o1 的推理链（[GitHub 仓库](https://github.com/tcsenpai/multi1)）

**	**•**	**thinkR：在 R 中用本地 LLM 实现类似 o1 的链式思考（[GitHub 仓库](https://github.com/eonurk/thinkR)）

**致谢**

此应用由 [Benjamin Klieger](https://x.com/benjaminklieger) 开发。
