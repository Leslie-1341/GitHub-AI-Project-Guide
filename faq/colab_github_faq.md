# AI 开发者高频问题速查

**1. 如何在 Colab 中安全地克隆我的私有 GitHub 仓库？**

**痛点**：直接写账号密码不安全，经常报错。
**方案**：使用 GitHub Personal Access Token (PAT)。
```python
# 在 Colab 单元格中运行：
# 注意：不要把带有 Token 的 notebook 直接保存公开！
!git clone https://<你的用户名>:<你的Token>@[github.com/](https://github.com/)<你的用户名>/<仓库名>.git
```

**2. 模型权重文件（如 .safetensors）超过 100MB，Git 拒绝推送怎么办？**

**痛点**：Git 默认不支持超过 100MB 的单文件。 **方案**：使用 Git LFS (Large File Storage)。

Bash

```
# 1. 本地安装并初始化 LFS
git lfs install
# 2. 追踪大文件类型
git lfs track "*.safetensors"
git lfs track "*.pth"
# 3. 正常 add, commit, push
git add .gitattributes
git add your_model.safetensors
git commit -m "Upload model weights"
git push origin main
```