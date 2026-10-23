# 数分智学伴 · 数学分析智能教学智能体

面向大学《工科数学分析》课程的“教学评一体化”学习智能体：会讲、会引导、会出题、会批改，并给出学习诊断与下一步练习。支持**文字、图片、PDF** 三种输入。

> 课程大作业 C1 作品本体（前端静态版 + 模型直连）。

## 在线访问

- **GitHub Pages（公开直链）**：https://mzxg1nishuihan-del.github.io/shufen-zhixueban-agent/
- 源码仓库：https://github.com/mzxg1nishuihan-del/shufen-zhixueban-agent
- 原智能体后端（备用）：https://shufen-zhixueban-agent.jauntyotter128.chatgpt.site

打开即用、无需登录。

## 功能

- **会讲**：概念探究 / 模块“进入练习”按五段式讲解（直观 → 定义/条件 → 例题 → 常见错误+反例 → 自测题）
- **会引导**：苏格拉底式提问 + 三级提示 H1/H2/H3（方向 → 关键步 → 完整框架）
- **会出题**：分层出题（基础/中档/挑战），附答案、解析、评分点、易错点、难度依据
- **会批改**：评分表步骤分、错误类型、正确解法、追问；前端自动提取得分并记录
- **教学评闭环**：讲 → 练 → 评 → 诊 → 推
- **学习诊断**：正确率、作答次数、模块掌握度、错误类型分布、最薄弱知识点、推荐 3 题
- **图片 / PDF 输入**：
  - 图片：自动压缩后作为多模态输入发给模型（识图、读题、看手写步骤）
  - PDF：浏览器端用 pdf.js 抽取文字；扫描版自动渲染页面为图片走视觉识别（最多 8 页）
  - 支持点击上传、拖拽、剪贴板粘贴图片
- **学习档案**：localStorage 持久化；支持导出对话记录与学习记录（Markdown）
- **题库**：30 道母题 × 100 条练习，支持知识点筛选、搜索、显示更多
- **API 设置**：页面右上角“API 设置”可改 Key / Base URL / 模型（仅保存在当前浏览器）
- **响应式**：桌面 / 移动端；MathJax 双 CDN；XSS 防护

## 技术架构

```
浏览器（GitHub Pages 静态页面）
   ├─ 主通道：直接调用 DeepSeek API（/chat/completions，含图片输入）
   └─ 备用：调用原 Sites Worker 的 /api/chat（纯文本，跨域）
```

- 前端是**单文件** `index.html`（HTML/CSS/JS 内联），无需构建。
- 系统提示词与六种模式提示词内置在前端（与原 Worker 一致）。
- `worker/index.js` 是原服务端源码备份（Sites 部署用，`/api/chat`、`/api/health`）。

## ⚠️ API Key 与安全说明（务必阅读）

本项目采用**前端直连模型**的方式，默认 API Key 写在 `index.html` 里。这意味着：

1. **任何访问者都能从网页源码中拿到这个 Key**，并可能盗用额度；
2. 请务必使用**限额 Key / 单独 Key**，不要用主账号大额度 Key；
3. 演示/答辩结束后建议到模型平台**吊销或轮换**该 Key；
4. 更安全的正式做法：把 Key 放在自己的服务端（Cloudflare Worker / Vercel / 服务器），前端只调用你的代理接口，不暴露 Key。本仓库的 `worker/index.js` 可直接改造为代理。

页面右上角“API 设置”支持随时替换 Key / Base URL / 模型，无需改代码。

## 本地预览

```bash
python -m http.server 8080
# 打开 http://127.0.0.1:8080/
```

图片/PDF 上传与模型调用需要浏览器允许跨域；建议用本地 HTTP 服务，不要直接双击 `file://`。

## 部署到 GitHub Pages

1. Fork / clone 本仓库；
2. 推送到 `main`；
3. Settings → Pages → Source 选 `Deploy from a branch` → `main` / `/ (root)`；
4. 1–2 分钟后访问 `https://<用户名>.github.io/shufen-zhixueban-agent/`。

`.nojekyll` 已包含，避免静态资源被 Jekyll 处理。

## 隐私与学术说明

- 不收集真实姓名、学号；对话、附件、学习记录仅保存在浏览器内存/localStorage（附件原图不落盘）。
- 图片/PDF 内容会发送给模型服务商用于推理，请勿上传含个人隐私的材料。
- 本项目为课程作业展示；数学内容引用教材通用结论，提交前请自行复核。
- 公式渲染使用 MathJax（Apache-2.0）；PDF 解析使用 pdf.js（Apache-2.0）。
