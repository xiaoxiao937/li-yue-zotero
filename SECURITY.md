# 狸阅安全说明

最后审查：2026-08-13（v1.0.0）

## 安全边界

Zotero 插件运行在 Zotero 客户端内，具有读取资料库、PDF 和创建笔记所需的权限。狸阅只应从本仓库、GitHub Release 或可信的中文社区镜像安装。

## v1.0.0 发布审查

发布前已对源代码与最终 XPI 执行以下检查：

- JavaScript 静态语法检查和核心逻辑测试；
- XUL/XML 结构检查；
- XPI 压缩包完整性检查；
- 安装包文件清单检查；
- 网络地址与请求代码检查；
- 动态代码执行、系统命令和子进程关键字检查；
- 文件写入、凭据访问、遥测、跟踪和广告 SDK 检查；
- 在线安装包回读及 SHA-256 校验。

v1.0.0 安装包仅包含 manifest、启动脚本、偏好、界面代码、图标、README、隐私和安全说明。未发现动态 `eval`、系统 Shell、子进程、遥测、广告或第三方分析 SDK。

这些检查能提供可复核的发布证据，但任何软件审计都不能承诺不存在未知漏洞。发现安全问题时，请通过 GitHub Issues 联系维护者；报告中请勿包含 API Key 或私人 PDF 原文。

## 网络访问

- 用户设置的 DeepSeek、OpenAI 或兼容 API 地址；
- GitHub `update.json` 与 XPI 地址，用于插件自动更新。

狸阅没有开发者运营的中转服务器。

## 本机访问

- 读取用户主动加入任务的 Zotero 条目和 PDF 提取文本；
- 创建对应条目的子笔记和 `🎶` 标签；
- 通过 Zotero 登录凭据机制保存 API Key；
- 在 Zotero 数据目录保存狸阅任务状态和 `.bak` 备份。

## 发布完整性

每个版本发布时应记录 XPI 的 SHA-256。用户可在 macOS/Linux 上执行：

```sh
shasum -a 256 li-yue-1.0.0.xpi
```

Windows PowerShell 可执行：

```powershell
Get-FileHash .\li-yue-1.0.0.xpi -Algorithm SHA256
```
