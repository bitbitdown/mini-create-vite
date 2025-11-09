# 🚀 自动发布配置指南

## ✅ 当前状态

- ✅ 已手动发布 v0.1.0
- ✅ publish.yml 已配置
- ⏳ 等待配置 NPM_TOKEN

---

## 🔑 第 1 步：生成 npm Access Token

### 1.1 访问 npm token 页面

```
https://www.npmjs.com/settings/bitbitdown/tokens
```

### 1.2 生成新 token

1. 点击 **"Generate New Token"**
2. 选择 **"Classic Token"**
3. **Token type**: 选择 **"Automation"** ⚠️ 重要！
4. 点击 **"Generate Token"**

### 1.3 复制 token

- ⚠️ **只显示一次**，务必保存
- 格式：`npm_xxxxxxxxxxxxxxxxxxxxxx`

---

## 🔐 第 2 步：添加到 GitHub Secrets

### 2.1 打开 GitHub Secrets 页面

```
https://github.com/bitbitdown/mini-create-vite/settings/secrets/actions
```

### 2.2 添加 Secret

1. 点击 **"New repository secret"**
2. 填写：
   - **Name**: `NPM_TOKEN` （必须是这个名字）
   - **Secret**: 粘贴你的 npm token
3. 点击 **"Add secret"**

### 2.3 验证

- 回到 Secrets 页面
- 应该看到 `NPM_TOKEN` 已添加
- 值会显示为 `***`

---

## 🎯 第 3 步：测试自动发布

### 3.1 更新版本号

```bash
# 确保在 main 分支
git checkout main

# 拉取最新代码
git pull origin main

# 更新版本号（会自动创建 git tag）
npm version patch  # 0.1.0 → 0.1.1

# 或者
npm version minor  # 0.1.0 → 0.2.0
```

### 3.2 推送代码和标签

```bash
# 推送代码
git push origin main

# 推送标签（触发自动发布）
git push origin --tags
```

### 3.3 查看 Actions 运行

1. 访问：https://github.com/bitbitdown/mini-create-vite/actions
2. 应该看到 "Publish to npm" 工作流正在运行
3. 等待完成（约 1-2 分钟）

---

## 📋 自动发布流程说明

### 触发条件

```yaml
on:
  push:
    tags:
      - "v*"  # 推送 v 开头的 tag 时触发
```

### 执行流程

```
npm version patch
    ↓
创建 tag: v0.1.1
    ↓
git push --tags
    ↓
GitHub Actions 检测到新 tag
    ↓
运行 publish.yml
    ↓
1. 检出代码
2. 安装依赖
3. 发布到 npm（使用 NPM_TOKEN）
4. 创建 GitHub Release
    ↓
完成！✅
```

---

## 🎬 完整操作示例

### 日常开发流程

```bash
# 1. 在 dev 分支开发
git checkout dev
# ...开发代码...
git add .
git commit -m "feat: add new feature"
git push origin dev

# 2. 合并到 main
git checkout main
git merge dev
git push origin main

# 3. 发布新版本
npm version patch  # 或 minor/major
git push origin main
git push origin --tags

# 4. 自动发布到 npm ✨
# 5. 自动创建 GitHub Release ✨
```

---

## 📊 版本号规则

```bash
# 当前版本：0.1.0

# 补丁版本（bug 修复）
npm version patch  # → 0.1.1

# 次版本（新功能，向后兼容）
npm version minor  # → 0.2.0

# 主版本（破坏性更新）
npm version major  # → 1.0.0
```

---

## 🔍 查看发布结果

### npm 包页面
```
https://www.npmjs.com/package/@bitbitdown/mini-create-vite
```

### GitHub Releases
```
https://github.com/bitbitdown/mini-create-vite/releases
```

### Actions 日志
```
https://github.com/bitbitdown/mini-create-vite/actions
```

---

## ⚠️ 注意事项

### 1. 不要手动创建 tag

❌ **错误**：
```bash
git tag v0.1.1
git push origin v0.1.1
```

✅ **正确**：
```bash
npm version patch  # 自动创建 tag
git push origin --tags
```

### 2. 确保在 main 分支发布

```bash
# 发布前确认分支
git branch
# * main  ✅

# 如果不在 main 分支
git checkout main
git pull origin main
```

### 3. 推送时要包含 tags

```bash
# 只推送代码，不会触发发布
git push origin main  ❌

# 要推送 tags 才能触发
git push origin --tags  ✅

# 或者一次性推送
git push origin main --follow-tags  ✅
```

---

## 🛠️ 故障排查

### 问题 1：Actions 没有运行

**检查：**
- tag 是否推送成功：`git ls-remote --tags origin`
- tag 格式是否正确：必须是 `v*`（如 v0.1.1）

### 问题 2：发布失败，提示权限错误

**检查：**
- NPM_TOKEN 是否正确添加
- Token 类型是否是 "Automation"
- Token 是否过期

### 问题 3：CI 测试失败

**解决：**
- 在本地先运行 `npm test` 确保通过
- 检查 CI 日志找到具体错误

---

## 🎯 快速检查清单

配置完成后，确认：

- [ ] NPM_TOKEN 已生成
- [ ] NPM_TOKEN 已添加到 GitHub Secrets
- [ ] publish.yml 文件存在
- [ ] 本地测试通过（`npm test`）
- [ ] 在 main 分支

然后可以测试自动发布：

```bash
npm version patch
git push origin --tags
```

---

## 🎉 现在开始

1. **立即配置**：生成 NPM_TOKEN 并添加到 GitHub
2. **测试发布**：运行 `npm version patch` 测试流程
3. **查看结果**：访问 Actions 页面查看运行状态

配置好后，以后发布新版本只需要 2 个命令：
```bash
npm version patch
git push origin --tags
```

就这么简单！🚀
