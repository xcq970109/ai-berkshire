阶段 0：项目盘点与备份
[ ] 确认 ai-berkshire 当前目录、Git 分支和未提交的改动
[ ] 创建独立的 dsh-migration 分支或其他可恢复的备份
[ ] 检查 skills/、codex-skills/、codex-prompts/、tools/ 和 reports/ 的作用
[ ] 阅读 README 与 AGENTS.md，整理当前项目对路径和工具的要求
[ ] 确定迁移后继续保留原始 Claude Code / Codex 工作流

阶段 1：Intel Mac 环境准备
[ ] 确认 uname -m 输出 x86_64，避免误用 Apple Silicon 专用安装包
[ ] 检查 Node.js、npm、Python 3 和 Git 的安装情况
[ ] 确认 Node.js 是 Intel x64 版本，并满足 DSH 当前运行要求
[ ] 将项目放在 ~/ai-berkshire，或者检查并修正脚本中的固定路径
[ ] 确认 Python 财务工具所需依赖能够安装或已有可用环境

阶段 2：部署 DSH Web UI
[ ] 通过 npm 安装或运行 DSH CLI
[ ] 启动 DSH Web UI，在浏览器中打开交互界面
[ ] 在 DSH 中打开 ai-berkshire 根目录作为工作区
[ ] 使用空白或临时测试目录做一次只读测试，验证基本对话和文件读取
[ ] 确认会话保存、文件访问和工具审批行为符合预期

阶段 3：接入 OpenCode Go
[ ] 安装并审查 dsh-opencode-go 插件的来源和权限
[ ] 在 DSH 的设置中配置 Go 订阅凭据，不把密钥提交到 Git
[ ] 刷新模型目录，核对实际可用的模型 ID
[ ] 验证普通文本响应和流式输出
[ ] 测试一次工作区文件读取或其他低风险工具调用
[ ] 确认订阅用量显示和模型错误提示是否正常

阶段 4：迁移 Codex Skills
[ ] 运行 scripts/sync-codex-skills.py，重新生成 codex-skills/
[ ] 将各个包含 SKILL.md 的 Skill 包复制到项目的 .agents/skills/
[ ] 保留 skills/ 和 codex-skills/，不要手工覆盖生成源
[ ] 检查每个 Skill 的 name、description、目录名和 frontmatter
[ ] 检查引用的参考文档、脚本、模板等是否一起复制
[ ] 先测试轻量 Skill，验证 DSH 确实发现并加载内容
[ ] 记录 Slash Command 与原生 Skill 调用方式之间的差异

阶段 5：Python 财务工具适配
[ ] 运行 tools/financial_rigor.py --help，确认 CLI 入口正常
[ ] 执行项目 README 提供的市值精确计算示例
[ ] 检查脚本使用的相对路径、固定路径、依赖和文件读写位置
[ ] 验证 Decimal 精确计算与关键数值校验逻辑
[ ] 确认 DSH 的执行环境和权限允许调用这些脚本
[ ] 修复必要的路径或环境问题，不随意改写财务算法
[ ] 将脚本执行结果和异常情况记录下来

阶段 6：完成单公司投研闭环
[ ] 选定一家上市公司作为固定测试样本
[ ] 使用轻量 Skill 测试资料搜集与任务参数解析
[ ] 运行 investment-research 相关工作流
[ ] 验证来源、数据截止日期、财务口径和估值假设
[ ] 确认 Python 校验确实执行，而非仅在报告中声称执行
[ ] 检查最终报告的保存位置、文件名和内容完整性
[ ] 确认旧报告没有被无意覆盖

阶段 7：多 Agent、多模型调度
[ ] 盘点 DSH 当前版本实际支持的 Subagent、Agent Teams 和工作流接口
[ ] 识别 investment-team 中的 Claude Code 专属工具调用
[ ] 将研究流程拆成资料研究、财务分析和独立审查三个职责
[ ] 为每个子任务指定已验证可用的 Go 模型
[ ] 先验证单个子 Agent 能启动、执行工具并返回结果
[ ] 验证多个独立研究任务能否并行执行
[ ] 检查每个子任务实际使用的模型、结果回传与失败处理
[ ] 由主 Agent 汇总分歧并完成报告审查

阶段 8：回归测试与部署固化
[ ] 重新执行基础 Skill、财务工具、单公司投研和多 Agent 测试
[ ] 检查模型不可用、工具失败和网络中断时的行为
[ ] 编写一份 Intel Mac 环境的部署文档
[ ] 记录 DSH、插件和运行时版本，以及必要的配置步骤
[ ] 整理迁移分支和代码差异，删除无用的临时文件
[ ] 验证从备份或新工作区恢复时的关键步骤
