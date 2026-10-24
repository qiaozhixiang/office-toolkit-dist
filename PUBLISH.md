# 发布到 GitHub 的步骤

这个目录已经是**脱敏**的插件源码，可以直接作为仓库根目录。

## 1. 先改成你自己的信息

| 位置 | 当前占位 | 改成 |
| --- | --- | --- |
| `_manifest.json` → `author.name` | `your-name` | 你的 GitHub 用户名 |
| `_manifest.json` → `author.url` | `https://github.com/your-name` | 你的主页 |
| `_manifest.json` → `urls.repository` | `https://github.com/your-name/maibot-office-toolkit` | 仓库地址 |
| `_manifest.json` → `urls.issues` | 同上 + `/issues` | 仓库 issues 地址 |
| `LICENSE` → `Copyright (c) 2026 your-name` | `your-name` | 你的名字 |
| `README.md` 里的 `your-name` 链接 | 2 处 | 你的仓库地址 |

## 2. 本地初始化并提交

```bash
cd office-toolkit
git init
git config user.name "你的名字"
git config user.email "you@example.com"
git add .
git commit -m "feat: 办公文件工具箱 v1.2.0"
git branch -M main
git remote add origin https://github.com/你的用户名/maibot-office-toolkit.git
git push -u origin main
```

## 3. 打 release 包（可选）

MaiBot 支持「从 ZIP 安装」，ZIP 里顶层目录必须是插件目录名：

```bash
python - <<'PY'
import zipfile
from pathlib import Path
root = Path("office-toolkit")
out = Path("dist/office-toolkit-1.2.0.zip")
out.parent.mkdir(exist_ok=True)
with zipfile.ZipFile(out, "w", zipfile.ZIP_DEFLATED) as z:
    for f in sorted(root.rglob("*")):
        if f.is_file() and "__pycache__" not in f.parts:
            z.write(f, f"office-toolkit/{f.relative_to(root).as_posix()}")
print(out, out.stat().st_size)
PY
```

## 4. 截图

`docs/screenshot.md` 里写了怎么补截图。截图前把窗口里的真实路径、QQ 号打码。

## 5. 检查清单

- [ ] `_manifest.json` 里没有 `your-name`
- [ ] `config.toml` 的 `qq_numbers` / `admins` 为空
- [ ] 全仓库搜不到自己的 QQ 号、内网 IP、绝对路径
- [ ] `python -c "import ast,pathlib;[ast.parse(p.read_text(encoding='utf-8')) for p in pathlib.Path('.').glob('*.py')]"`
- [ ] 用 MaiBot 的「从 ZIP 安装」试装一次
