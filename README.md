# 数分智学伴 · 数学分析智能教学智能体

面向大学《工科数学分析》课程的“教学评一体化”学习智能体：会讲、会引导、会出题、会批改，并给出学习诊断与下一步练习。

> 课程大作业 C1 作品本体（前端静态版）。智能体后端由 OpenAI Sites 的 `/api/chat` 提供，前端通过 HTTPS 跨域调用，已实测可用。

## 在线访问

- **GitHub Pages（公开直链）**：https://mzxg1nishuihan-del.github.io/shufen-zhixueban-agent/
- 智能体原站（后端）：https://shufen-zhixueban-agent.jauntyotter128.chatgpt.site
- 源码仓库：https://github.com/mzxg1nishuihan-del/shufen-zhixueban-agent

> 打开即用、无需登录。前端为静态页面，聊天由原站 Worker 的 `/api/chat` 提供（跨域已实测可用）。

## 功能

- **会讲**：概念探究 / 模块“进入练习”按五段式讲解（直观 → 定义/条件 → 例题 → 常见错误+反例 → 自测题）
- **会引导**：苏格拉底式提问 + 三级提示 H1/H2/H3（方向 → 关键步 → 完整框架）
- **会出题**：分层出题（基础/中档/挑战），附答案、解析、评分点、易错点、难度依据
- **会批改**：评分表步骤分、错误类型、正确解法、追问；前端自动提取得分并记录
- **教学评闭环**：讲 → 练 → 评 → 诊 → 推
- **学习诊断**：正确率、作答次数、模块掌握度、错误类型分布、最薄弱知识点、推荐 3 题
- **学习档案**：localStorage 持久化；支持导出对话记录与学习记录（Markdown）
- **题库**：30 道母题 × 100 条练习，支持知识点筛选、搜索、显示更多
- **响应式**：桌面 / 移动端；MathJax 双 CDN；XSS 防护

## 技术架构

```
浏览器（本仓库 index.html，GitHub Pages 静态托管）
   │  POST /api/chat（跨域，HTTPS）
   ▼
OpenAI Sites Worker（worker/index.js，服务端持有模型 Key）
   │  OpenAI 兼容 /chat/completions
   ▼
大模型
```

- 前端是**单文件** `index.html`（HTML/CSS/JS 内联），无需构建。
- `worker/index.js` 是后端源码备份：包含六种模式的系统提示词与 `/api/chat`、`/api/health`。
- 前端把 API 地址写死为 Sites 域名（见 `index.html` 中 `fetch('https://shufen-zhixueban-agent…/api/chat'`）。如后端迁移，改这一处即可。

## 本地预览

```bash
python -m http.server 8080
# 打开 http://127.0.0.1:8080/
```

跨域调用需要浏览器允许；直接双击 `index.html`（file://）可能被 CORS 拦截，请用本地 HTTP 服务。

## 部署到 GitHub Pages

1. 新建公开仓库（如 `shufen-zhixueban-agent`）；
2. 推送本目录到 `main`；
3. 仓库 Settings → Pages → Source 选 `Deploy from a branch` → `main` / `/ (root)`；
4. 等待 1–2 分钟，访问 `https://<用户名>.github.io/shufen-zhixueban-agent/`。

`.nojekyll` 已包含，避免静态资源被 Jekyll 处理。

## 隐私与学术说明

- 不收集真实姓名、学号；对话与学习记录仅保存在浏览器 localStorage。
- 本项目为课程作业展示；数学内容引用教材通用结论，提交前请自行复核。
- 引用的字体/公式渲染使用 MathJax（Apache-2.0）；其余代码为课程作业原创。
