---
name: line-by-line-chinese-comments
description: Add beginner-friendly Chinese comments to project source code line by line while preserving behavior. Use when the user asks to add Chinese comments, annotate files or a whole project inline, explain code in-source for beginners, make code readable for novices, or uses phrases like 逐行注释, 中文注释, 给初学者看懂, or 项目代码注释.
---

# 逐行中文注释

## 目标

在源码中补充适合初学者阅读的中文注释，让读者能顺着代码逐行理解“这行在做什么、为什么要这样做、它和项目功能有什么关系”。

## 工作流程

1. 先确认注释范围：
   - 如果用户指定了文件、目录或模块，只处理指定范围。
   - 如果用户要求处理整个项目，先用 `rg --files` 查看结构，并识别入口文件、核心业务文件和主要语言。
   - 如果项目很大，优先按模块推进；当范围超过约 15 个源码文件或 1500 行有效代码时，先请用户确认处理顺序。

2. 跳过不适合直接加注释的内容：
   - 跳过依赖、产物、缓存和生成文件，如 `.git`, `node_modules`, `vendor`, `dist`, `build`, `.next`, `coverage`, `.venv`, `target`, `__pycache__`。
   - 跳过 lock 文件、minified 文件、source map、二进制文件、图片、字体、压缩包。
   - 不要给标准 JSON 文件加注释，因为会破坏格式；如果用户要求解释 JSON，改为输出说明或在用户同意后创建单独的 Markdown 讲解文件。
   - 配置文件只注释支持注释语法且对初学者有价值的关键项。

3. 添加注释前先读懂代码：
   - 先阅读待修改文件及其相邻调用关系，弄清入口、数据流、状态变化和副作用。
   - 不要只根据单行字面意思机械注释；注释要结合项目上下文说明用途。

4. 按语言使用原生注释语法：
   - Python, Shell, YAML, TOML: 使用 `#`。
   - JavaScript, TypeScript, Java, Go, Rust, C, C++, C#, Kotlin, Swift: 使用 `//` 或必要时使用块注释。
   - HTML 和 Vue template: 使用 `<!-- ... -->`。
   - JSX/TSX: 在标签内部使用 `{/* ... */}`，在普通代码区域使用 `//`。
   - CSS/SCSS: 使用 `/* ... */`。
   - SQL: 使用 `--` 或 `/* ... */`。

5. 保持“逐行”但避免破坏可读性：
   - 默认给每一行有实际含义的源码添加相邻中文注释，包括 import、变量、函数、条件、循环、调用、返回值和异常处理。
   - 可以跳过空行、已有注释、纯括号或纯分隔符行、重复数据行、闭合标签行。
   - 简短语句优先使用行尾注释；长表达式、多行语句或嵌套结构优先在语句上方添加注释。
   - 对复杂代码块，先加一条块级说明，再给关键行补逐行说明。
   - 如果已有英文注释，保留原注释；必要时在旁边补充中文解释，不要删除有效信息。

6. 注释内容要面向初学者：
   - 用自然中文解释具体作用，例如“保存用户输入的邮箱，后面提交表单时会用到”，不要写空泛的“定义变量”。
   - 第一次出现框架概念、设计模式或缩写时，用一句话解释，例如 middleware、hook、promise、DTO、ORM。
   - 说明重要分支、边界条件、错误处理和副作用。
   - 不要翻译变量名、函数名、API 名称、字符串常量或文件路径。
   - 不要添加 `console.log`、调试代码、示例代码或会改变运行行为的内容。

7. 修改时保持代码行为不变：
   - 不改变逻辑、缩进层级、导入顺序、公开 API、配置值或格式化风格。
   - 不在多行表达式中插入会导致语法错误的注释。
   - 不为了容纳注释随意换行；如果需要换行，先确认该语言的语法允许。
   - 使用小范围补丁逐文件修改，避免牵动无关内容。

## 验证

完成后尽量运行与语言匹配的验证：

- Python: `python -m py_compile` 或项目测试。
- JavaScript/TypeScript: `npm run lint`, `npm run typecheck`, `npm test`, `npm run build` 中可用且合适的命令。
- Go: `go test ./...`。
- Rust: `cargo test`。
- Java/Kotlin/C#/C++ 等项目：使用现有构建或测试命令。

如果无法运行验证，明确说明原因，并至少重新打开抽样文件检查注释语法。

## 输出格式

任务完成后用中文简要汇报：

```markdown
## 已添加中文注释
- `path/to/file.ext`：说明处理范围

## 跳过或未处理
- `path/to/file.ext`：说明原因

## 验证
- 已运行：命令和结果
- 未运行：原因
```
