# AI Skill 合集

这是一组面向 AI 编程助手（如 Claude、CodeBuddy 等）的专业技能（Skill）集合，涵盖算法艺术、视觉设计、文档处理、前端开发、3D 渲染等多个领域。每个 Skill 以 Markdown 格式编写，可读性好，便于维护和扩展。

## 技能列表

### 🎨 algorithmic-art — 算法艺术生成

使用 p5.js 创建生成式算法艺术，支持种子随机性和交互式参数探索。适用于用户需要用代码创作艺术、生成艺术、流场、粒子系统等场景。输出包括算法哲学文档（.md）和交互式生成艺术查看器（.html + .js），强调原创性，避免抄袭现有艺术家作品。

### 🎯 brand-guidelines — 品牌视觉规范

应用 Anthropic 官方品牌颜色和排版规范到任何需要 Anthropic 风格的作品中。提供品牌主色、强调色、字体（Poppins/Lora）的完整定义和智能应用规则。适用于需要品牌配色、样式指南、视觉格式化或企业设计标准的场景。

### 🖼️ canvas-design — 画布视觉设计

基于设计哲学创建精美的视觉艺术作品（.png/.pdf）。采用两步工作流：先创建设计哲学（.md），再将其在画布上视觉表达。支持多页创作、字体选择和精细化打磨，追求博物馆/杂志级品质。适用于海报、艺术品、设计稿等静态视觉作品创作。

### 🤖 claude-api — Claude API 开发

使用 Claude API 或 Anthropic SDK 构建 LLM 驱动的应用。支持 Python、TypeScript、Java、Go、Ruby、C#、PHP 等多种语言，涵盖单次调用、流式响应、工具调用、Agent SDK、批量处理、文件上传等完整功能。内置模型选择、思考模式、缓存优化等最佳实践指南。

### 📝 doc-coauthoring — 文档协作撰写

引导用户通过结构化工作流协作撰写文档。三阶段流程：1) 上下文收集 — 用户提供背景信息，AI 提出澄清问题；2) 精炼与结构 — 逐节头脑风暴、筛选、起草和迭代修改；3) 读者测试 — 用无上下文的新 AI 实例测试文档可读性，发现盲点。适用于技术规范、提案、决策文档等各类文档撰写。

### 📄 docx — Word 文档处理

创建、读取、编辑和分析 Word 文档（.docx）。新建文档使用 docx-js（支持标题、列表、表格、图片、目录、页眉页脚、超链接、脚注等），编辑现有文档采用解包→编辑 XML→重新打包的方式，支持修订追踪和批注。适用于报告、备忘录、信函、模板等专业文档生成和处理。

### 💻 frontend-design — 前端界面设计

创建独特的、生产级的前端界面，强调高设计品质和避免千篇一律的 AI 审美。在设计前进行设计思考（目的、风格、约束、差异化），注重排版、色彩、动效、空间构图和背景细节。适用于网页组件、落地页、仪表盘、React 组件、HTML/CSS 布局等前端 UI 构建场景。

### 🌐 threejs-skills — Three.js 3D 开发

Three.js 相关的完整技能集合，涵盖 3D 开发的各个方面：

| 子技能 | 说明 |
|--------|------|
| `threejs-fundamentals` | 场景搭建、相机、渲染器、Object3D 层级、坐标系统 |
| `threejs-geometry` | 几何体创建与操作、自定义几何体、BufferGeometry |
| `threejs-materials` | 材质系统、PBR 材质、材质属性与效果 |
| `threejs-textures` | 纹理加载、UV 映射、纹理属性与动画 |
| `threejs-lighting` | 光照系统、阴影、环境光、点光源、聚光灯等 |
| `threejs-animation` | 动画系统、关键帧动画、骨骼动画、变形动画 |
| `threejs-interaction` | 用户交互、射线检测、拖拽、鼠标/触摸事件 |
| `threejs-loaders` | 模型加载器、GLTF/OBJ/FBX 等格式导入 |
| `threejs-shaders` | 自定义着色器、GLSL 编程、ShaderMaterial |
| `threejs-postprocessing` | 后处理效果、Bloom/SSAO/色调映射等 |

## 项目结构

```
ai-skill/
├── algorithmic-art/          # 算法艺术生成
├── brand-guidelines/         # 品牌视觉规范
├── canvas-design/            # 画布视觉设计
├── claude-api/               # Claude API 开发
├── doc-coauthoring/          # 文档协作撰写
├── docx/                     # Word 文档处理
├── frontend-design/          # 前端界面设计
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

每个 Skill 目录下的 `SKILL.md` 文件是该技能的核心定义文件，包含技能名称、描述、触发条件和使用指南。将 Skill 安装到 AI 编程助手中后，助手会根据用户的需求自动匹配和加载对应技能。

## 协议

各 Skill 的许可协议详见其目录下的 `LICENSE.txt` 文件。
