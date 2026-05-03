# omni-tools Prompts

> 项目：iib0011/omni-tools
> 技术栈：React + Vite + TypeScript + Tailwind CSS，i18next 国际化，工具分类：图片/PDF/字符串/视频/音频/JSON/CSV/时间/数字/XML/转换器

---

## 功能迭代

**1. 添加工具收藏夹功能**
在 omni-tools 中为用户添加工具收藏功能。用户可以点击工具卡片上的星标图标将工具加入收藏，收藏列表保存到 localStorage，在首页顶部显示"我的收藏"分区，方便快速访问常用工具。

**2. 添加工具使用历史记录**
在 omni-tools 中记录用户最近使用的工具（最多 10 条），保存到 localStorage。在首页添加"最近使用"分区，显示工具图标和名称，点击直接跳转，提升重复使用效率。

**3. 支持工具批量处理模式**
在 omni-tools 的图片和 PDF 工具中添加批量处理功能。用户可以一次上传多个文件，工具对每个文件执行相同操作后，提供打包下载（ZIP）。在工具界面添加"批量模式"切换开关。

**4. 添加工具分享功能**
在 omni-tools 中为每个工具添加"分享"按钮，生成包含当前工具配置参数的 URL（通过 URL hash 编码），用户可以将配置好的工具链接分享给他人，对方打开后自动恢复配置状态。

**5. 添加工具使用统计面板**
在 omni-tools 的管理页面添加工具使用统计，展示各工具的使用次数排行、最受欢迎的工具分类、每日活跃用户数。数据通过前端 localStorage 本地统计，无需后端支持。

---

## Bug 修复

**6. 修复大文件处理时浏览器卡顿**
在 omni-tools 中处理超过 50MB 的图片或 PDF 文件时，主线程被阻塞导致页面卡顿。请将文件处理逻辑迁移到 Web Worker 中执行，通过 postMessage 与主线程通信，保持 UI 响应流畅。

**7. 修复工具搜索不支持中文关键词**
在 omni-tools 的工具搜索框中，输入中文关键词时无法找到对应工具（工具名称为英文）。请在工具定义中添加中文别名字段，搜索时同时匹配英文名称和中文别名。

**8. 修复 PDF 工具在 Safari 中不可用**
在 omni-tools 中，PDF 相关工具在 Safari 浏览器中无法正常工作，显示空白或报错。请检查 PDF.js 的 Safari 兼容性配置，确保使用兼容的 API 和 Worker 加载方式。

**9. 修复视频转换工具在移动端内存溢出**
在 omni-tools 的视频处理工具中，在移动端处理视频文件时因内存限制导致崩溃。请添加文件大小检测，在移动端限制最大处理文件大小，并显示友好的提示信息。

**10. 修复工具页面刷新后语言设置丢失**
在 omni-tools 中，用户切换语言后刷新页面，语言设置恢复为默认值。请将语言偏好保存到 localStorage，页面加载时优先读取 localStorage 中的语言设置。

---

## 重构

**11. 将工具定义统一为声明式配置**
omni-tools 中各工具的定义方式不统一，部分工具有额外的配置字段。请统一工具定义的 Schema（使用 Zod 验证），确保所有工具都包含必填字段（id/name/description/category/component），并在构建时验证。

**12. 将文件处理工具提取为共享 Hook**
omni-tools 中多个工具（图片压缩、PDF 合并、视频转换）都有相似的文件上传、进度显示、下载逻辑。请提取通用的 `useFileProcessor` Hook，减少重复代码，统一错误处理和进度反馈。

---

## 测试

**13. 为字符串工具编写单元测试**
使用 Vitest 为 omni-tools 的字符串处理工具（Base64 编解码、URL 编解码、哈希计算、大小写转换等）编写单元测试，覆盖正常输入、空输入、特殊字符、超长字符串等边界情况。

**14. 为工具路由编写集成测试**
使用 Playwright 为 omni-tools 编写端到端测试，覆盖：首页工具列表加载、工具搜索、按分类筛选、进入工具页面、执行基本操作（如 Base64 编码）、验证输出结果。

**15. 为 i18n 翻译完整性编写测试**
为 omni-tools 编写翻译完整性检查脚本，对比所有语言文件与英文基准文件，找出缺失的翻译 key，并在 CI 中运行此检查，防止新增工具时遗漏翻译。

---

## 代码理解

**16. 解释 omni-tools 的工具注册机制**
在 omni-tools 中，新工具是如何注册到系统中的？`defineTool` 函数的作用是什么？工具的路由是如何自动生成的？如何添加一个新的工具分类？

**17. 解释 omni-tools 的 i18n 架构**
在 omni-tools 中使用了 i18next 实现多语言支持。请解释语言文件的组织结构（按工具分命名空间？）、工具名称和描述如何国际化、如何在工具组件内部使用翻译函数。

---

## DevOps

**18. 编写 GitHub Actions 多架构构建流水线**
为 omni-tools 编写 `.github/workflows/docker-build.yml`，实现推送 main 分支时自动构建多架构（amd64/arm64）Docker 镜像并推送到 Docker Hub，使用 npm 缓存加速构建。

**19. 编写 Nginx 配置优化**
为 omni-tools 的 Nginx 部署添加性能优化配置：开启 gzip 压缩（针对 JS/CSS/HTML）、设置静态资源缓存头（1年）、配置 SPA 路由回退（try_files）、添加安全响应头（CSP/X-Frame-Options）。

**20. 编写 docker-compose.yml 部署配置**
为 omni-tools 编写 `docker-compose.yml`，包含：omni-tools 服务（映射 80 端口）、健康检查（curl localhost/health）、自动重启策略、可选的 HTTPS 配置（通过环境变量切换）。

---

## 构建与截图命令

**构建截图：**
```bash
cd /path/to/omni-tools && docker build -t omni-tools-test .
```

**网页截图：**
```bash
docker run -d -p 8080:80 --name omni-tools-test omni-tools-test && sleep 3 && open http://localhost:8080
```

**清理：**
```bash
docker rm -f omni-tools-test && docker rmi omni-tools-test
```
