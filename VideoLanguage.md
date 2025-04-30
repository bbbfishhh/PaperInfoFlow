# 多模态，所谓的多模态

## 视频理解的 LLM 汇总：

这里都是指普通用户可调用 API 的模型。

1. Gemini 全系列

不管是2.0, 2.5 不管是 flash，还是 pro 都可以视频理解 + 音频理解，强无敌多模态。

官方文档：https://ai.google.dev/gemini-api/docs/video-understanding?hl=zh-cn#python_3

免费部分：

您可以通过以下方式将视频作为输入提供给 Gemini：

- 使用 File API 上传视频文件，然后向 generateContent 发出请求。对于大于 20MB 的文件、时长超过大约 1 分钟的视频，或者您想在多个请求中重复使用文件时，请使用此方法。
- 将请求中的内嵌视频数据传递给 generateContent。适用于文件较小（小于 20 MB）且时长较短的视频。
- 直接在问题中添加 YouTube 网址。

所以可以长视频剪切成 20 MB去尝试，有能力的直接付费吧。

2. Qwen 多模态

按照 token 收费

Qwen-VL 是视觉理解，而不是视频理解，所以只能看，不能听。

QvQ 视觉推理，有点意思。

qwen-vl-ocr 文字提取 OCR

qwen-audio-turbo-latest 音频理解

Qwen-Omni 全模态： 视频 + 音频 + 文字

https://help.aliyun.com/zh/model-studio/user-guide/multimodal/?spm=a2c4g.11186623.help-menu-2400256.d_0_2.14241d46GQo1Hw

3. glm 系列

按照 token 收费

glm 模型巨多，但是杂乱无章，自己弃用的也多。

深挖下去，视频理解是 glm-4v-plus-0111，估计是自带 audio 的

https://bigmodel.cn/dev/api/normal-model/glm-4v

4. 上海ai lab 开源的 InternVL3

InternVL3-78B 多模态大模型，而且开源！号称首个性能比肩 GPT-4o, Gemini Pro 等顶尖闭源模型的开源模型。

震撼了，

https://internlm.intern-ai.org.cn/api/document