# Ex-ChatGPT - ChatGPT with ToolFormer

![language](https://img.shields.io/badge/language-python-blue) ![GitHub](https://img.shields.io/github/license/circlestarzero/EX-chatGPT) ![GitHub last commit](https://img.shields.io/github/last-commit/circlestarzero/EX-chatGPT) ![GitHub Repo stars](https://img.shields.io/github/stars/circlestarzero/EX-chatGPT?style=social)

简体中文 [English](./README.en.md) / [Background](./BACKGROUND.md)

ChatGPT 是一个强大的工具平台，可以无需任何调整就生成 API 请求来协助回答问题。`Ex-ChatGPT` 使得 ChatGPT 能够调用外部 API，例如 **WolframAlpha、Google 和 WikiMedia**，以提供更准确和及时的答案。

这个项目分为 `Ex-ChatGPT` 和 `WebChatGPTEnhance` 两部分。前者是一个使用了 `GPT3.5 Turbo API`、**WolframAlpha、Google 和 WikiMedia** 等 API 的服务，能够提供更强大的功能和更准确的答案。后者是一个**浏览器扩展程序**，它更新了原有的 WebChatGPT 插件以支持添加外部 API，支持 ChatGPT 网页调用不同的 API 和提示。

## 交互界面

### ExChatGPT

![chatHistory](img/newPage.jpg)

### WebChatGPTEnhance

![WebChatGPT](img/chatGPTChromeEnhance.png)

## Highlights

-   **docker 和 proxy 支持**
-   **聊天记录冗余备份**
-   支持 OpenAI GPT-3.5 Turbo API
-   允许 ChatGPT 调用外部 API 接口 ( **Google,WolframAlpha,WikiMedia** )
-   自动保存载入对话历史，**自动压缩对话**
-   **可显示使用的 Token 数量**
-   **openAI api key 池**
-   **Markdown and MathJax** 渲染
-   调用**API 过程显示动画**, 类似必应
-   **历史对话管理**载入，类 chatgpt 页面布局
-   **快捷键**快速选择模式 `Tab` 和换行 `Shift+Enter`,`Enter` 发送， `up`,`down` 选择历史发送消息，类似终端
-   `stream` 特性，它类似于打字机的效果，可以更快地响应结果。与一次性加载所有内容不同，stream会逐步输出结果。如示例中所示：
![stream](img/stream.gif)
-   `chat` 模式下**prompt 自动补全**选择，支持模糊搜索， 拼音搜索， 支持自定义 prompt, 项目中自带 [awesome-chatgpt-prompts](https://github.com/f/awesome-chatgpt-prompts) 中的 `prompt`
![promptCompletion](img/promptCompletion.gif)

## 安装

### Ex-chatGPT Installation

-   `pip install`
`pip install -r requirements.txt`
-   将 `apikey.ini.example` 复制改名为 `apikey.ini`,然后在 `apikey.ini` 中填入你的 API 密钥， 以及代理 ( 如果只有一个 `openAI` 的 `API key`,将 `key1 = sk-xxxx; key2 = sk-xxxx` 删除即可 )
  -   `Google api key and search engine id` [申请](https://developers.google.com/custom-search/v1/overview?hl=en)
  -   `wolframAlpha app id key` [申请](https://products.wolframalpha.com/api/)
  -   `openAI api key`( 新功能 ) 或 `chatGPT access_token` ( 旧版本 ) [申请](https://platform.openai.com)
-   运行 `main.py` 并打开 `http://127.0.0.1:1234/`
-   选择模式 ( 可以使用 `Tab` ) ，例如 `chat,detail,web,webDirect,WebKeyWord`
-   `chat` 模式下 使用 `\{promptname} {query}` 格式来模糊搜索选择 prompt
-   **快捷键**快速选择模式 `Tab` 和换行 `Shift+Enter`,`Enter` 发送， `up`,`down` 选择历史发送消息，类似终端

#### Docker 快速部署

```bash
# 克隆代码
git clone https://github.com/circlestarzero/EX-chatGPT.git
# 进入项目目录
cd EX-chatGPT/chatGPTEx
# 将配置补充完整
cp apikey.ini.example apikey.ini
# 配置补充完整后启动
docker compose up -d

# 访问
http://your_ip:5000

# 查看日志
docker logs -f --tail 100 exchatgpt
```

### WebChatGPTEnhance Installation

-   在 `chatGPTChromeEhance/src/util/apiManager.ts/getDefaultAPI` 中填入 Google API 信息
-   运行 `npm install`
-   运行 `npm run build-prod`
-   在 `chatGPTChromeEhance/build` 中获取构建好的扩展
-   add your `prompts` and `APIs` in option page.
  -   `APIs` and `prompts` examples are in `/WebChatGPTAPI`
  -   `wolframAlpha` needs to run local sever - `WebChatGPTAPI/WolframLocalServer.py`

## 模式介绍

### Web Mode

Web Mode 开始时会直接询问 ChatGPT 一个问题。ChatGPT 会生成一系列与查询相关的 API 调用，并使用第一个返回的结果和问题进行验证和补充。最后，ChatGPT 会对信息进行总结。Web Mode 具有比仅总结响应更好的聊天能力。

### Chat Mode

Chat Mode 仅调用 OpenAI API 接口，类似于 ChatGPT 的 Web 版本。您可以通过输入 `/promtname` 来搜索和选择不同的提示，它还支持模糊搜索。

### WebDirect Mode

WebDirect Mode 首先让 ChatGPT 生成一系列与查询相关的 API 调用。然后，它直接调用第三方 API 搜索每个查询的答案，最后 ChatGPT 对信息进行总结。WebDirect Mode 对于单个查询信息更快且相对更准确。

### Detail Mode

Detail Mode 是 WebDirect Mode 的扩展，它会进行额外的 API 调用来补充当前结果中未找到的信息 ( 例如之前未搜索到的信息 ) 。最后，ChatGPT 对信息进行总结。

### Keyword Mode

Keyword Mode 直接从 ChatGPT 中生成关键词进行查询，使用 DDG 进行查询，不需要其他 API 密钥。但是其准确性相对较差。

## 更新日志

-   `stream` 特性，它类似于打字机的效果，可以更快地响应结果。与一次性加载所有内容不同，stream会逐步输出结果。如示例中所示：
![stream](img/stream.gif)
-   聊天记录冗余备份
-   chat 模式下 prompt 自动补全选择，支持模糊搜索和拼音搜索

![promptCompletion](img/promptCompletion.gif)

-   更新 Docker 和 proxy 支持
-   支持 OpenAI GPT-3.5 Turbo API，快速且价格低廉
-   提供额外的 API 调用和搜索摘要，以提供更全面和详细的答案
-   使用快捷键快速选择模式 `Tab` 和换行 `Shift+Enter`，同时使用 `Enter` 发送消息。使用 `up` 和 `down` 选择历史发送消息，类似终端操作
-   更新历史对话管理，支持载入、删除和保存历史对话

![chatHistory](img/newPage.jpg)

-   更新 API 调用处理动画

![APIAnimation](img/APIAnimation.png)

-   页面美化

![WebBeautification](img/WebPageBeautification.jpg)

-   Markdown 和 MathJax 渲染器

![MathJax](img/mathjax.jpg)

-   更新聊天记录 token 优化器，Web 模式可以根据聊天记录进行响应；添加 token 成本计数器

![history](img/webHistory.jpg)

-   更新 Web 聊天模式选择，优化 prompt 和 token 成本，限制 token 上限

![mode](img/mode.jpg)

-   改进对中文查询的支持，并添加当前日期信息

![date](img/date.jpg)

-   更新 Web 聊天模式并修复一些错误
-   更新 API 配置
-   更新 API 池
-   自动保存载入对话历史，ChatGPT 可联系之前对话
