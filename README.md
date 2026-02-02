# AlsoPro 远程配置文件

这个目录包含 AlsoPro 的远程配置文件，用于控制客户端行为。

## 使用方法

### 方式一：GitHub 托管（推荐）

1. **Fork 或创建新仓库**
   - 在 GitHub 创建一个新仓库，例如 `alsopro-config`
   - 将此目录下的所有 JSON 文件上传到仓库

2. **修改客户端配置**
   - 编辑 `src/core/remote_urls.py`
   - 修改 `GITHUB_REPO` 为你的仓库地址：
   ```python
   GITHUB_REPO = "https://raw.githubusercontent.com/你的用户名/alsopro-config/main"
   ```

3. **更新配置**
   - 直接在 GitHub 网页上编辑 JSON 文件
   - 或者本地修改后 push 到仓库
   - 客户端会自动获取最新配置

### 方式二：Gitee 托管（国内推荐）

1. 在 Gitee 创建仓库并上传文件
2. 使用 Gitee 的 Raw 地址：
   ```python
   GITHUB_REPO = "https://gitee.com/你的用户名/alsopro-config/raw/main"
   ```

### 方式三：其他静态托管

可以使用任何支持静态文件的服务：
- 阿里云 OSS
- 腾讯云 COS
- 七牛云
- Cloudflare R2

---

## 配置文件说明

| 文件 | 用途 | 更新频率 |
|------|------|----------|
| `version.json` | 版本控制、强制更新 | 发布新版本时 |
| `config.json` | 功能开关、使用限制 | 随时 |
| `models.json` | 可用模型列表 | 添加新模型时 |
| `providers.json` | Provider 配置、维护通知 | 随时 |
| `announcements.json` | 公告推送 | 随时 |
| `workflows.json` | ComfyUI 工作流 | 添加新工作流时 |
| `user_groups.json` | 用户分组权限 | 调整权限时 |
| `featured.json` | 推荐内容、热门提示词 | 随时 |
| `content_filter.json` | 敏感词过滤 | 随时 |
| `pricing.json` | 定价/积分配置 | 调整价格时 |
| `integrity.json` | 文件完整性校验 | 每次发布时 |

---

## 常用操作示例

### 1. 发布新版本

编辑 `version.json`：
```json
{
    "version": "1.0.1",
    "build": 101,
    "download_url": "https://github.com/.../releases/download/v1.0.1/AlsoPro-1.0.1.zip",
    "changelog": "- 修复问题\n- 新增功能",
    "force_update": false
}
```

### 2. 强制更新（发现严重 Bug）

```json
{
    "version": "1.0.1",
    "force_update": true,
    "min_version": "1.0.1"
}
```

### 3. 维护某个服务

编辑 `providers.json`：
```json
{
    "providers": {
        "sora": {
            "enabled": false,
            "maintenance": true,
            "notice": "Sora 服务维护中，预计 2 小时后恢复",
            "maintenance_end": "2026-02-01T14:00:00Z"
        }
    }
}
```

### 4. 发布公告

编辑 `announcements.json`：
```json
{
    "announcements": [
        {
            "id": "ann-001",
            "title": "重要通知",
            "content": "系统将于今晚维护...",
            "type": "warning",
            "expires_at": "2026-02-02T00:00:00Z"
        }
    ]
}
```

### 5. 禁用某个功能

编辑 `config.json`：
```json
{
    "feature_flags": {
        "batch_generation.enabled": false
    }
}
```

### 6. 添加新模型

编辑 `models.json`，在对应 provider 下添加：
```json
{
    "id": "new-model",
    "name": "新模型",
    "enabled": true,
    "is_default": false,
    "is_new": true
}
```

---

## 注意事项

1. **JSON 格式**：确保 JSON 格式正确，可以用在线工具验证
2. **缓存**：GitHub Raw 有缓存，更新后可能需要几分钟生效
3. **向后兼容**：新增字段时保持向后兼容，旧客户端应能正常解析
4. **敏感信息**：不要在公开仓库放敏感信息

---

## 客户端更新频率

| 配置 | 更新时机 |
|------|----------|
| version | 启动时 |
| config | 启动时 + 每小时 |
| models | 启动时 + 每小时 |
| providers | 启动时 + 每30分钟 |
| announcements | 启动时 + 每30分钟 |
| workflows | 手动刷新 |
| featured | 启动时 |
