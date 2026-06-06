---
name: read-project-modules
description: Analyze an unfamiliar codebase for a beginner reader by identifying only independent functional modules, their responsibilities, and the main technologies used. Use when the user wants to understand someone else's project structure, divide it into learnable modules, or output a concise module list while ignoring development and build tooling unless the project itself is about those tools.
---

# Project Module Reader

## Goal

Help a beginner understand an unfamiliar project by dividing the codebase into independent functional modules that can be studied one by one.

## Workflow

1. Inspect the repository structure first:
   - Read top-level directories and key manifests such as `package.json`, `pyproject.toml`, `requirements.txt`, `go.mod`, `Cargo.toml`, `pom.xml`, or similar files when present.
   - Identify application entry points, routes, services, packages, and major data or integration boundaries.

2. Identify modules by function, not by every folder:
   - Treat a module as an independently meaningful part of the product or system.
   - Merge small folders when they support the same feature or runtime responsibility.
   - Avoid splitting by technical layers alone unless those layers are clearly independent modules.

3. Determine the main technologies for each module:
   - Include frameworks, runtimes, storage systems, protocols, SDKs, and domain-specific libraries used by that module.
   - Ignore development tools and build tools such as Webpack, Vite, ESLint, Prettier, test runners, bundlers, and CI tools unless the project itself implements or centers on those tools.

4. Keep the explanation beginner-friendly:
   - Use clear module names.
   - Prefer practical labels such as `Frontend UI`, `Backend API`, `Search Pipeline`, `MCP Service`, `Database Layer`, or names that match the project domain.
   - Do not over-explain implementation details unless needed to justify the module boundary.

## Output Format

Always output in this format:

```markdown
## 模块列表

1. **模块名称**：[例如：前端UI、后端API、检索管道、MCP服务]
   - 使用的主要技术：[例如：React, Tailwind CSS]
2. **模块名称**：...
   - 使用的主要技术：...
```

## Rules

- Only split the project into independent functional modules.
- Ignore development tools and build tools unless the project itself is about them.
- If a technology is inferred from file names or conventions rather than explicit dependencies, state it as an inference.
- If the project is too small for multiple modules, return one module and briefly name why it is a single functional unit.
