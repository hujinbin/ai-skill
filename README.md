# AI Skill 合集

这是一组面向 AI 编程助手的专业技能（Skill）集合，覆盖算法艺术、视觉设计、文档处理、演示文稿、表格处理、前端开发、网页测试、3D 渲染、内部沟通、PDF 处理、MCP 服务构建等多个领域。每个 Skill 都以 Markdown 编写，便于阅读、维护和扩展。

## 技能列表

### algorithmic-art - 算法艺术生成

使用 p5.js 创建生成式算法艺术，支持种子随机性和交互式参数探索。适用于用代码创作艺术、生成流场、粒子系统等场景。输出通常包含算法哲学文档和交互式生成艺术查看器，强调原创性，避免仿照现有艺术家作品。

### brand-guidelines - 品牌视觉规范

将 Anthropic 官方品牌颜色和排版规范应用到需要 Anthropic 风格的作品中。提供品牌主色、强调色、字体（Poppins/Lora）定义和智能应用规则，适用于品牌配色、样式指南、视觉格式化或企业设计标准场景。

### canvas-design - 画布视觉设计

基于设计哲学创建精美的静态视觉作品（PNG/PDF）。采用两步工作流：先创建设计哲学文档，再在画布上视觉表达。适用于海报、艺术品、设计稿等静态视觉创作。

### claude-api - Claude API 开发

使用 Claude API 或 Anthropic SDK 构建 LLM 驱动应用。覆盖 Python、TypeScript、Java、Go、Ruby、C#、PHP 等语言，包含单次调用、流式响应、工具调用、Agent SDK、批量处理、文件上传、模型选择、思考模式和缓存优化等实践。

### doc-coauthoring - 文档协作撰写

引导用户通过结构化流程协作撰写文档。流程包括上下文收集、精炼与结构化、读者测试，适用于技术规范、提案、决策文档等需要多轮打磨的文档。

### docx - Word 文档处理

创建、读取、编辑和分析 Word 文档（.docx）。新建文档使用 docx-js，支持标题、列表、表格、图片、目录、页眉页脚、超链接、脚注等；编辑现有文档采用解包、编辑 XML、重新打包的方式，支持修订跟踪和批注。

### frontend-design - 前端界面设计

创建独特、生产级的前端界面，强调高设计品质，避免千篇一律的 AI 审美。适用于网页组件、落地页、仪表盘、React 组件、HTML/CSS 布局等前端 UI 构建场景。

### internal-comms - 内部沟通文档

帮助撰写各类内部沟通文档，使用公司常见格式。支持 3P 更新、公司通讯、FAQ 回复、状态报告、管理层更新、项目更新、事件报告等多种文档类型。

### mcp-builder - MCP 服务器构建

创建高质量 MCP（模型上下文协议）服务器的完整指南，让 LLM 能通过精心设计的工具与外部服务交互。支持 Python（FastMCP）和 TypeScript（MCP SDK），覆盖工具设计、命名规范、分页、错误处理、安全性等实践。

### pdf - PDF 文件处理

提供全面的 PDF 处理能力，支持读取和提取文本/表格、合并拆分、旋转页面、添加水印、创建新 PDF、填写表单、加密/解密、提取图片、OCR 扫描版 PDF 等。使用 pypdf、pdfplumber、reportlab、qpdf、pdftotext 等工具。

### pptx - PowerPoint 演示文稿处理

处理任何与 `.pptx` 相关的任务，包括创建演示文稿、读取或提取文本、编辑现有幻灯片、使用模板、合并或拆分文件、处理布局、演讲者备注和批注。提供三类核心流程：

- 读取分析：使用 `markitdown` 提取文本，使用缩略图脚本查看视觉概览，必要时解包 XML。
- 基于模板编辑：遵循 `editing.md` 中的解包、编辑、清理、重新打包流程。
- 从零创建：遵循 `pptxgenjs.md` 使用 pptxgenjs 生成完整幻灯片。

该 Skill 特别强调演示设计质量：为每页加入图像、图表、图标或形状等视觉元素，选择符合主题的配色和字体组合，并通过内容检查、视觉检查和渲染验证循环发现并修复问题。

### skill-creator - Skill 创建与优化

用于创建新 Skill、修改现有 Skill、运行评估并优化触发描述。它提供从需求访谈、撰写 `SKILL.md`、设计测试提示、运行带 Skill 与基线对比、生成评审页面、收集反馈、迭代改进到最终打包的完整工作流。

适用于从零创建技能、优化已有技能、衡量技能效果、分析结果方差，以及改进 frontmatter 中 `description` 的触发准确性。该 Skill 还包含评估结果查看器、打包脚本、描述优化脚本和评审代理说明。

### slack-gif-creator - Slack 动态 GIF 制作

为 Slack 优化动态 GIF 的知识和工具包。支持创建 128x128 的表情 GIF 或 480x480 的消息 GIF，提供帧生成、GIF 优化、Slack 适配验证、缓动函数和常见动画概念。

该 Skill 使用 PIL 绘制图形并通过 `GIFBuilder` 组装和优化输出，适合制作抖动、脉冲、弹跳、旋转、淡入淡出、滑入、缩放、粒子爆发等动画效果。它强调图形应有足够的视觉细节、对比度和动感，而不是简单占位图形。

### theme-factory - 主题工厂

为演示文稿、文档、报告、HTML 落地页等产物应用统一专业主题。内置 10 套主题，每套包含配色、字体组合和视觉风格说明，并提供 `theme-showcase.pdf` 供用户预览选择。

内置主题包括 Ocean Depths、Sunset Boulevard、Forest Canopy、Modern Minimalist、Golden Hour、Arctic Frost、Desert Rose、Tech Innovation、Botanical Garden 和 Midnight Galaxy。若预设主题不适合，也可根据用户输入创建新的自定义主题。

### web-artifacts-builder - Web Artifact 构建

用于创建复杂的 claude.ai HTML artifacts，技术栈包括 React 18、TypeScript、Vite、Tailwind CSS、shadcn/ui 和 Parcel。适合需要状态管理、路由、多组件结构或 shadcn/ui 组件的复杂前端 artifact，不适合简单单文件 HTML/JSX。

工作流包括初始化项目、开发 artifact、将代码打包成单个自包含 HTML 文件、展示给用户，并在必要时进行测试。该 Skill 也强调避免常见 AI 风格问题，例如过度居中、紫色渐变、统一圆角和默认 Inter 字体。

### webapp-testing - Web 应用测试

使用 Python Playwright 脚本与本地 Web 应用交互并进行测试。适用于验证前端功能、调试 UI 行为、捕获浏览器截图、查看浏览器日志和自动化页面操作。

该 Skill 提供 `scripts/with_server.py` 管理本地服务生命周期，支持单服务或多服务启动。推荐先等待动态应用达到 `networkidle`，再检查 DOM、截图、发现选择器并执行交互，避免在 JavaScript 尚未渲染完成时误判页面状态。

### xlsx - Excel 与表格文件处理

处理以电子表格为主要输入或输出的任务，包括读取、编辑、修复 `.xlsx`、`.xlsm`、`.csv`、`.tsv` 文件，添加列、计算公式、格式化、制图、清洗数据、创建新工作簿或在表格格式之间转换。

该 Skill 强调交付专业级 Excel 文件：使用一致字体、保持模板原有风格、避免公式错误，并在财务模型中遵循行业配色规范。对计算类内容优先使用 Excel 公式而非硬编码结果，必要时通过 LibreOffice 重新计算并检查 `#REF!`、`#DIV/0!`、`#VALUE!`、`#NAME?` 等错误。

### threejs-skills-main - Three.js 3D 开发

Three.js 相关技能集合，覆盖 3D 开发的多个方面：

| 子技能 | 说明 |
| --- | --- |
| `threejs-fundamentals` | 场景搭建、相机、渲染器、Object3D 层级、坐标系统 |
| `threejs-geometry` | 几何体创建与操作、自定义几何体、BufferGeometry |
| `threejs-materials` | 材质系统、PBR 材质、材质属性与效果 |
| `threejs-textures` | 纹理加载、UV 映射、纹理属性与动画 |
| `threejs-lighting` | 光照系统、阴影、环境光、点光源、聚光灯等 |
| `threejs-animation` | 动画系统、关键帧动画、骨骼动画、变形动画 |
| `threejs-interaction` | 用户交互、射线检测、拖拽、鼠标与触摸事件 |
| `threejs-loaders` | 模型加载器、GLTF/OBJ/FBX 等格式导入 |
| `threejs-shaders` | 自定义着色器、GLSL 编程、ShaderMaterial |
| `threejs-postprocessing` | 后处理效果、Bloom、SSAO、色调映射等 |

## 项目结构

```text
ai-skill/
├── algorithmic-art/          # 算法艺术生成
├── brand-guidelines/         # 品牌视觉规范
├── canvas-design/            # 画布视觉设计
├── claude-api/               # Claude API 开发
├── doc-coauthoring/          # 文档协作撰写
├── docx/                     # Word 文档处理
├── frontend-design/          # 前端界面设计
├── internal-comms/           # 内部沟通文档
├── mcp-builder/              # MCP 服务器构建
├── pdf/                      # PDF 文件处理
├── pptx/                     # PowerPoint 演示文稿处理
├── skill-creator/            # Skill 创建与优化
├── slack-gif-creator/        # Slack 动态 GIF 制作
├── theme-factory/            # 主题工厂
├── web-artifacts-builder/    # Web Artifact 构建
├── webapp-testing/           # Web 应用测试
├── xlsx/                     # Excel 与表格文件处理
├── threejs-skills-main/      # Three.js 3D 开发
│   ├── threejs-animation/
│   ├── threejs-fundamentals/
│   ├── threejs-geometry/
│   ├── threejs-interaction/
│   ├── threejs-lighting/
│   ├── threejs-loaders/
│   ├── threejs-materials/
│   ├── threejs-postprocessing/
│   ├── threejs-shaders/
│   └── threejs-textures/
└── README.md
```

## 使用方式

每个 Skill 目录下的 `SKILL.md` 文件是该技能的核心定义文件，包含技能名称、触发描述、使用条件和操作指南。将 Skill 安装到支持技能机制的 AI 编程助手后，助手会根据用户需求自动匹配并加载对应技能。

部分 Skill 还包含配套资源：

- `scripts/`：可执行脚本，用于处理确定性或重复性任务。
- `references/`：补充文档，按需读取。
- `assets/`：模板、示例、字体、图标或其他输出资源。
- `examples/`：示例脚本或参考用法。

## 协议

各 Skill 的许可协议请查看对应目录下的 `LICENSE.txt` 文件。
