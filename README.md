# 狸阅 for Zotero

狸阅使用同一个自定义 Prompt 分别阅读多篇 Zotero PDF，并把每篇 AI 回答保存为对应文献的独立子笔记。它不会把多篇文献合并总结，也不会把 Prompt 写进生成的笔记。

狸阅是个人维护的免费、非商业公益工具。插件本身不收费、不销售 API、不提供付费中转服务；用户仅需向自己选择的 AI 服务商支付可能产生的 API 费用。

## 主要功能

- 每篇文献独立调用 AI、独立生成笔记；
- 支持从 Zotero 当前选中项载入，也可在狸阅中跨资料库和文件夹继续添加文献；
- 支持 DeepSeek、OpenAI Responses API 和自定义 OpenAI Chat Completions 兼容接口；
- 支持模型列表读取、DeepSeek 官方价格更新、API 余额、并发档位、费用与耗时预估、取消和失败重试；
- 长 PDF 自动分块处理，并检查模型是否完整结束；
- 普通 PDF 按物理页提取文本，AI 回答中的关键事实会尽量附带 `[PDF p.页码]`，便于回到原文核查；
- 相同 Prompt 与相同 PDF 已生成过笔记时默认跳过，避免重复处理；
- 成功笔记标题以“🐱 狸阅”开头，并为父文献添加 `🎶` 标签；
- 打开狸阅笔记并在正文末尾停留后，自动为父文献添加 `✅` 已读标签；
- 在条目树中直接按 `1`～`5` 给所选文献添加一至五星标签，按 `0` 清除评级；
- 支持任务报告、完成通知、历史报告、草稿恢复和异常中断恢复；
- 可选的扫描版 PDF OCR：通过用户单独配置的 OpenAI PDF 视觉接口读取扫描件，默认关闭；
- 支持通过公开更新清单自动升级。

## 兼容性

- 狸阅 1.3.1：Zotero 9.0.x、10.0.x
- 已在 macOS Zotero 9.0.6 和 10.0.6 中完成兼容性检查；Zotero 10.0.6 已完成隔离启动验证。
- Windows 和 Linux 尚需更多社区测试，欢迎通过 Issues 反馈。

## 安装

1. 从 [GitHub Releases](https://github.com/xiaoxiao937/li-yue-zotero/releases/latest) 下载最新的 `.xpi` 文件。
2. 在 Zotero 中打开“工具 → 插件”。
3. 点击齿轮菜单，选择“Install Plugin From File…”并选择下载的 XPI。
4. 点击工具栏的小猫；无需预先选择文献，空白启动时可直接使用“添加文献”。

从 v0.9.18 起，狸阅支持 Zotero 自动更新。插件升级只替换插件代码，不会主动删除本机 API 设置、草稿、恢复任务或历史报告。

## 快速使用

1. 点击工具栏小猫打开狸阅。
2. 使用 Zotero 主界面当前所选文献，或点击“添加文献”从其他文件夹继续加入。
3. 输入自己的 Prompt。
4. 在“API 设置”中选择服务商、模型并填写自己的 API Key。
   - 如需处理扫描版 PDF，在同一窗口开启“OCR”，并填写独立的 OpenAI OCR 配置；随后重检 PDF。
5. 查看预计费用与耗时后开始处理。
6. 每篇成功回答会立即保存到对应文献的子笔记。

## 已读标识与评级

- `🎶` 表示该文献已经生成过狸阅笔记。
- `✅` 表示你打开了狸阅笔记，并在正文末尾停留至少约 3 秒。该规则只是一种可解释的阅读进度标识，并不声称能够判断是否真正理解了笔记。
- 在 Zotero 主界面的条目树中选中文献后，直接按 `1`～`5` 可设置一至五星评级，按 `0` 清除评级；也可使用右键菜单“狸阅：评级”。
- 新评级会替换旧的星级标签，不会删除条目的其他标签。输入框或可编辑文本区域处于焦点时，评级快捷键不会触发。

列表中至少有一篇文献时，狸阅会自动保存文献列表、Prompt、勾选状态和所选 PDF。零文献状态不会保存或提示草稿。

## API 设置

### DeepSeek

- 服务商：`DeepSeek`
- Base URL：`https://api.deepseek.com`
- 协议：Chat Completions

### OpenAI

- 服务商：`OpenAI`
- Base URL：`https://api.openai.com/v1`
- 协议：Responses API

输入 API Key 后可点击“读取模型”，从当前账户的 `/models` 接口同步可用模型；也可以手工填写模型 ID。界面中的模型价格和汇率只是估算参数，最终费用以 API 服务商账单为准。

DeepSeek 用户可在“任务预估”中查看官方余额接口返回的真实账户余额，并在“API 设置”中点击“更新价格”，从 DeepSeek 官方价格页抓取当前模型价格。当前内置价格（美元 / 1M tokens）如下：

- `deepseek-v4-flash`：缓存输入 `$0.0028`、未缓存输入 `$0.14`、输出 `$0.28`；
- `deepseek-v4-pro`：缓存输入 `$0.003625`、未缓存输入 `$0.435`、输出 `$0.87`。

OpenAI 官方的组织成本接口需要管理员密钥，普通 API Key 不提供“剩余余额”查询。狸阅不会要求或保存高权限管理员密钥，因此会明确显示不支持，而不会把用量估算冒充余额。自定义兼容接口同样不做未经声明的余额请求。

### 扫描版 PDF（OCR）

- OCR 默认关闭。开启后，只有被判定为缺少可用文本层的扫描件才会走 OCR，普通 PDF 仍由上方主模型处理。
- DeepSeek V4 是纯文本模型，不能直接读取扫描页面；狸阅因此提供独立的 OpenAI Responses PDF 视觉通道。主模型可以继续使用 DeepSeek。
- OCR Base URL、模型、API Key 和估算价格独立保存。OCR API Key 同样写入 Zotero 的本机登录凭据，不进入报告或笔记。
- 扫描件会以 PDF 文件形式发送给该 OCR 接口，由视觉模型直接完成本篇 Prompt。界面会显示 OCR 篇数、文件大小上限和近似费用；PDF 图像 tokens 的实际计费以服务商返回的 usage 为准。
- OCR 生成的笔记会在“生成信息”中标明读取方式和 OCR 模型。

## 隐私与数据处理

- 狸阅没有开发者运营的中转服务器、用户账户或遥测服务。
- 处理普通文献时，PDF 提取文本和 Prompt 会发送到用户自己配置的主 AI API；开启 OCR 后，扫描 PDF 文件和 Prompt 会发送到用户单独配置的 OpenAI OCR API。服务商如何保存和处理数据，以其隐私政策及用户账户设置为准。
- API Key 通过 Zotero 登录凭据机制保存在本机，不写入笔记、历史报告、草稿或普通偏好设置。
- Prompt、任务恢复状态和最近 20 次历史报告保存在本机 Zotero 数据目录；自动更新不会主动删除这些数据。
- 生成的笔记不显示原始 Prompt，只保存不可见指纹用于识别重复任务。
- 安装任何 Zotero 插件都等同于授予其访问 Zotero 数据和本机能力；请只从本仓库或可信镜像下载安装包。

更完整说明见 [PRIVACY.md](./PRIVACY.md)，发布安全边界和审计方法见 [SECURITY.md](./SECURITY.md)。

## 已知限制

- OCR 仅支持能够接受 Responses API `input_file` PDF 输入的 OpenAI 或兼容接口；DeepSeek 官方模型本身不支持视觉输入。
- 页码标注由 AI 根据插件提供的 PDF 物理页边界生成，适合快速定位，但仍应回到原文核对；若 Zotero 的逐页提取失败，狸阅会明确要求模型不要虚构页码。
- 一个文献条目在一次任务中只处理一个 PDF。
- 取消不会撤回已经发出的 HTTP 请求，但会停止启动后续文献任务。
- 自定义兼容接口必须支持 `POST /chat/completions`。
- 狸阅会同时激活 Zotero 应用和任务窗口；macOS 开启“台前调度”且狸阅位于另一个窗口组时，最终切换行为仍由系统窗口管理器决定。

## 问题反馈

请在 [GitHub Issues](https://github.com/xiaoxiao937/li-yue-zotero/issues) 提交问题。建议包含：Zotero 版本、操作系统、狸阅版本、复现步骤和已脱敏的错误信息。请勿提交 API Key、未公开 PDF 原文或其他隐私数据。

## 自动更新与开发验证

`./build.sh` 会生成 XPI 和 `update.json`。发布后，Zotero 通过以下更新清单检查新版：

```text
https://raw.githubusercontent.com/xiaoxiao937/li-yue-zotero/main/update.json
```

本项目无需 Node 构建步骤；Node 仅用于静态语法和纯逻辑测试：

```sh
node --check bootstrap.js
node --check content/main.js
node --check content/task.js
node tests/core.test.js
./build.sh
python3 scripts/verify_update_manifest.py update.json
```

## 许可证

当前仓库未授予开源许可证，默认保留全部权利。狸阅作为免费、非商业公益工具公开提供使用；这不代表获得复制、修改或重新发布源代码及安装包的许可。
