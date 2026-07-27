# qoder-skills

我个人沉淀的 Qoder Agent Skills 集合。每个子目录是一个独立技能，可单独取用。

## 技能列表

| 技能 | 说明 |
|---|---|
| [douyin-knowledge-animation](douyin-knowledge-animation/) | 抖音竖屏纯动画知识科普视频制作：把任意原理拆成"悬念封面 + 一句金句一页动画 + 结尾定格"的单文件 HTML 竖屏动画（720×1280），录屏后配卡点 BGM 发布 |

## 安装使用

把想要的技能文件夹复制到 Qoder 的技能目录即可：

```
# 个人技能（所有项目全局生效）
C:\Users\<你的用户名>\.qoder\skills\<技能名>\        (Windows)
~/.qoder/skills/<技能名>/                            (macOS / Linux)

# 或项目技能（仅当前项目生效）
<项目根目录>\.qoder\skills\<技能名>\
```

安装后两种触发方式：
- 自然语言：直接描述需求（如"做一个电梯调度的抖音知识动画"），Agent 自动匹配
- 斜杠命令：`/douyin-knowledge-animation 主题`

> 提示：不想占 C 盘的话，可以把技能真身放其他盘，再用目录联接指过去：
> `New-Item -ItemType Junction -Path "C:\Users\<用户名>\.qoder\skills\<技能名>" -Target "<实际路径>"`

## 技能文件结构说明

每个技能采用"入口 + 附录"的渐进式披露结构（SKILL.md 必读，附录按需加载），
设计逻辑详见各技能目录内的 README.md。
