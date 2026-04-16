# WellCorrelator — GitHub Actions 自动打包指南

## 文件结构

```
你的仓库/
├── .github/
│   └── workflows/
│       └── build.yml      ← GitHub Actions 工作流
├── v5.7.py                ← 你的主程序
├── requirements.txt       ← 依赖列表
└── README.md              ← 项目说明
```

---

## 第一步：初始化 Git 仓库

```bash
cd /path/to/your/project
git init
git add v5.7.py requirements.txt .github/workflows/build.yml README.md
git commit -m "Initial commit: WellCorrelator v5.7"
```

---

## 第二步：创建 GitHub 仓库并推送

### 方式 A：命令行（推荐）

```bash
# 1. 在 GitHub 网页上新建一个空仓库 (不要勾选 README)
# 2. 关联并推送
git remote add origin https://github.com/你的用户名/WellCorrelator.git
git branch -M main
git push -u origin main
```

### 方式 B：GitHub Desktop

直接拖拽文件夹到 GitHub Desktop 应用里。

---

## 第三步：触发自动构建

推送代码后，GitHub Actions **自动开始编译**（约 10–15 分钟）。

查看进度：

1. 打开 GitHub 仓库页面
2. 点击 **Actions** 标签
3. 点击正在运行的工作流查看日志

---

## 第四步：下载 exe

构建完成后，两种方式获取：

### 方式 1：Artifacts（每次 push 都生成）

1. 进入 **Actions** → 点击最新的 workflow run
2. 页面底部看到 **Artifacts** 区域
3. 点击 **WellCorrelator-Windows** 下载 zip
4. 解压得到 `WellCorrelator.exe`

### 方式 2：Release（手动触发时）

1. 进入仓库 → **Actions** → 左侧 **Build WellCorrelator**
2. 点击右上角 **Run workflow** → **Run workflow**
3. 等待完成后，进入 **Releases** 页面
4. 下载最新 Release 里的 exe

---

## 手动触发构建

不想等 push？随时手动跑：

1. 打开 GitHub 仓库
2. 点击 **Actions** → **Build WellCorrelator**
3. 点击 **Run workflow** → 绿色按钮
4. 等待 10–15 分钟
5. 下载 exe

---

## 更新版本号

每次发布新版本，修改 `build.yml` 里的版本号：

```yaml
--windows-product-version=5.8.0.0
```

然后 push，会自动触发新构建。

---

## 常见问题

### Q: 构建失败怎么办？

查看 **Actions** → 点击失败的 run → 看日志。常见原因：

- `v5.7.py` 有语法错误
- 依赖包版本冲突

### Q: 怎么加应用图标？

1. 把 `app.ico` 放到仓库根目录

2. 在 `build.yml` 的 nuitka 命令里加一行：

   ```yaml
   --windows-icon-from-ico=app.ico `
   ```

### Q: 构建时间太长？

正常，首次编译 PyQt6 + NumPy + Pandas 确实要 10–15 分钟。GitHub 免费额度足够用。

### Q: exe 文件在哪里永久保存？

- Artifacts 默认保留 30 天
- Release 永久保存（推荐正式发布时用）

---

## 完整文件清单（推送到 GitHub 所需）

```
✓ v5.7.py
✓ requirements.txt
✓ .github/workflows/build.yml
✓ README.md (可选)
✓ app.ico (可选，应用图标)
```

就这些，推上去等几分钟，exe 自动出来。
