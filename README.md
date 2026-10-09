# McD Breakfast Variety · 麦当劳早餐不重样 Skill

## 项目介绍

一个让每天早餐不重样的麦当劳点单 Skill。通过麦当劳 MCP（mcd-mcp）查询用户历史订单做去重，结合早餐时段菜单和当前星期，挑一份与近期不重复的早餐**套餐**并完成下单（支持到店自取 / 麦乐送）。核心是"不重样"——靠历史去重让你天天换花样，不用再纠结早上吃什么。

> 本项目为麦当劳程序员节创意开发大赛参赛作品，由参赛者独立开发，非麦当劳官方产品。

## 目标用户

- 每天吃麦当劳早餐、但懒得想吃什么的上班族 / 学生
- 容易连续几天点同一份早餐、想换花样的用户
- 希望一句话就完成"选店→选餐→算价→下单"全流程的效率党
- 任何已申请麦当劳 MCP Token、使用支持 MCP 的 AI 助手的用户

## 安装方法

本项目是一个基于麦当劳 MCP 的 Skill，运行在支持 MCP 的 AI 客户端中（如 Kiro、WorkBuddy 等）。

### 1. 申请麦当劳 MCP Token

参考 [麦当劳 MCP Server 使用指南](https://github.com/M-China/mcd-mcp-server) 登录并申请 MCP Token。

### 2. 配置 mcd-mcp MCP Server

在你的 AI 客户端 MCP 配置中加入（将 `YOUR_MCP_TOKEN` 替换为你申请到的 Token）：

```json
{
  "mcpServers": {
    "mcd-mcp": {
      "type": "streamablehttp",
      "url": "https://mcp.mcd.cn",
      "headers": {
        "Authorization": "Bearer YOUR_MCP_TOKEN"
      }
    }
  }
}
```

### 3. 安装 Skill

将本仓库的 `skills/mcd-breakfast-variety/SKILL.md` 放入你的 AI 客户端 skills 目录（例如 Kiro 的 `.kiro/skills/mcd-breakfast-variety/SKILL.md`），客户端会自动识别。

详细的 MCP 工具与调用流程见 [`MCP_INTEGRATION.md`](MCP_INTEGRATION.md)。

## 使用示例

安装好后，在对话框里直接说口令：

```text
我要麦当劳早餐不重样
```

可带参数：

```text
我要麦当劳早餐不重样 到店        # 到店自取
我要麦当劳早餐不重样 外送        # 麦乐送到家
我要麦当劳早餐不重样 预算20      # 控制预算（元）
我要麦当劳早餐不重样 清淡        # 口味 / 偏好提示
```

一次典型对话：

```text
你：我要麦当劳早餐不重样
Skill：📅 今天周五，还在早餐供应时段
       ♻️ 你最近 5 天点过「咸蛋黄鸡腿蛋月堡」，这次帮你换一份
       🍔 推荐：麦满分培根蛋堡超值套餐（麦满分 + 醇香奶咖 + 薯饼）
       💰 预估 ¥15.9（已用早餐券）
       确认下单请回复"下单"
你：下单
Skill：订单已创建，这是支付链接 …… 支付完成后回复"支付完成"我帮你查状态
```

## 核心逻辑

1. 获取当前时间，判断是否在早餐供应时段
2. 查历史订单做去重（避开最近 N 天点过的主餐）—— "不重样"的核心
3. 定位门店（到店自取 / 麦乐送）
4. 拉早餐菜单，**优先选套餐**
5. 算价（含可用优惠券）
6. 用户确认后下单，返回支付链接（支付由用户在麦当劳 App 完成）

详见 [`skills/mcd-breakfast-variety/SKILL.md`](skills/mcd-breakfast-variety/SKILL.md)。

## 信息安全

本仓库仅包含 Skill 定义，**不含任何真实 Token、密钥或账号凭证**，MCP 配置中的 Token 一律以 `YOUR_MCP_TOKEN` 占位符表示。
