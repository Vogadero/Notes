# SSH 密钥

在 Windows 10 上管理多平台 Git 账户的 SSH 密钥需要以下步骤，我会详细介绍整个过程：

---

## 🔑 一、检查现有 SSH 密钥
1. **打开 Git Bash**（安装 Git 时自带）
2. 输入命令检查现有密钥：
   ```bash
   ls -al ~/.ssh
   ```
   查看是否存在 `id_rsa`、`id_ed25519` 等文件（`.pub` 是公钥）

---

## 🔐 二、生成新 SSH 密钥（针对不同平台）
为每个平台生成独立密钥：
```bash
# 生成 GitHub 密钥
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_github -C "github_email@example.com"

# 生成 Gitee 密钥
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_gitee -C "gitee_email@example.com"

# 生成 AtomGit 密钥
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_atomgit -C "atomgit_email@example.com"

# 生成 GitLab 密钥
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_gitlab -C "gitlab_email@example.com"
```
- 执行时按 Enter 跳过密码设置（或设置安全密码）
- 密钥会保存在 `C:\Users\你的用户名\.ssh\`

---

## ⚙️ 三、创建 SSH 配置文件
1. 在 `.ssh` 目录创建 `config` 文件（无扩展名）
2. 添加以下内容：

```bash
# GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github
  IdentitiesOnly yes

# Gitee
Host gitee.com
  HostName gitee.com
  User git
  IdentityFile ~/.ssh/id_rsa_gitee
  IdentitiesOnly yes

# AtomGit
Host atomgit.com
  HostName atomgit.com
  User git
  IdentityFile ~/.ssh/id_ed25519_atomgit
  IdentitiesOnly yes

# GitLab
Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_rsa_gitlab
  IdentitiesOnly yes
```

---

## 📤 四、添加公钥到各平台
1. **查看公钥内容**：
   ```bash
   cat ~/.ssh/id_ed25519_github.pub
   ```
   复制全部内容（以 `ssh-ed25519` 或 `ssh-rsa` 开头）

2. **添加到各平台**：
   - **GitHub**：Settings → SSH and GPG keys → New SSH key
   - **Gitee**：设置 → SSH 公钥
   - **AtomGit**：个人设置 → SSH 公钥
   - **GitLab**：Preferences → SSH Keys

---

## ✅ 五、测试连接
分别测试各平台：
```bash
ssh -T git@github.com
ssh -T git@gitee.com
ssh -T git@atomgit.com
ssh -T git@gitlab.com
```
成功提示示例：
```
Hi username! You've successfully authenticated...
```

---

## 🔄 六、配置本地 Git 账户
为不同仓库设置对应账户：
```bash
# 全局设置（默认账户）
git config --global user.name "全局用户名"
git config --global user.email "全局邮箱"

# 为特定仓库设置
cd project-github
git config user.name "GitHub专用名"
git config user.email "github@example.com"
```

---

## 🛠️ 七、多账户切换问题解决
| **问题**                                | **解决方案**                                        |
| --------------------------------------- | --------------------------------------------------- |
| 权限错误 (Permission denied)            | 执行 `ssh-add ~/.ssh/对应密钥文件` 添加密钥到 agent |
| 提示 "Too many authentication failures" | 在 SSH 配置文件中添加 `IdentitiesOnly yes`          |
| 不同平台使用相同邮箱                    | 在 Git 本地配置中覆盖全局设置（仓库级配置）         |
| 连接超时                                | 检查防火墙设置，允许 SSH（端口 22）                 |

---

## 🔒 八、密钥管理最佳实践
1. **定期轮换密钥**：每 6-12 个月更新一次密钥
2. **使用密码保护**：生成密钥时设置密码（需配合 SSH agent）
3. **备份密钥**：将整个 `.ssh` 目录备份到加密存储
4. **查看已加载密钥**：
   ```bash
   ssh-add -l  # 查看已加载密钥
   ssh-add -D  # 清除所有加载密钥
   ```

---

## 💻 Windows 特别注意事项
1. 如果使用 PowerShell：
   ```powershell
   # 启动 ssh-agent
   Get-Service ssh-agent | Set-Service -StartupType Automatic
   Start-Service ssh-agent
   
   # 添加密钥
   ssh-add ~\.ssh\id_ed25519_github
   ```
2. 文件权限问题：
   - 右键点击密钥文件 → 属性 → 安全 → 编辑权限
   - 确保只有你的用户有完全控制权限

完成以上步骤后，您就可以在 Windows 10 上无缝切换不同 Git 平台的账户了。首次使用新密钥连接时可能需要确认主机指纹（输入 `yes` 即可）。