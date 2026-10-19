# metalabrasive-selection-AI

金属磨料智能选型计算器 — 公开站点：https://hannibalzhang007.github.io/metalabrasive-selection-AI/

**Proprietary — © 2026 Hannibal Zhang / FENGERDA. All rights reserved.** See [LICENSE](./LICENSE).

---

## 仓库结构

| 文件 | 说明 | 是否推送 GitHub |
|---|---|---|
| `index.html` | **发布版**（已压缩，无附录表格） | ✅ 推送 |
| `og-cover.png` | 社交分享封面 | ✅ 推送 |
| `.nojekyll` | 关闭 Jekyll 处理 | ✅ 推送 |
| `LICENSE` | 专有许可 | ✅ 推送 |
| `_protection_review/` | 开发版源码、分析文档、构建脚本、API 原型 | ❌ **不要推送** |
| `dist/index.html` | 构建产物（与发布版相同） | ❌ 无需推送 |

> ⚠️ `_protection_review/` 内含**未压缩的开发版源码与完整数据表**。推上去等于把第 1 层的成果全部作废。
> 推送前请确认 `.gitignore` 已排除该目录（见下）。

---

## 日常改代码的正确流程

```bash
# 1. 改开发版（未压缩，可读，有注释）
编辑 _protection_review/index_protected.html

# 2. 构建发布版（压缩 + 去注释 + 清附录数据渲染）
cd _protection_review
python build_minify.py            # 默认仅压缩，安全
python build_minify.py --obfuscate  # 可选混淆，⚠️ 需构建后复检 HTML

# 3. 用构建产物覆盖发布版
copy ..\dist\index.html ..\index.html

# 4. 提交推送
git add index.html LICENSE
git commit -m "Update"
git push
```

**永远不要直接编辑 `index.html`**——那是压缩产物，改起来痛苦且下次构建会被覆盖。

---

## 已实施的源代码保护（第 1+2 层）

1. **删除隐藏附录表格**（原 852–878 行）
   `display:none` 的 4 张表把 ABR/IND/MATRIX 数据渲染进 DOM，view-source 即可截图。
   现已连同 `renderTables` 死代码一并移除——**数据只参与计算，不写入 DOM**。

2. **LICENSE + 版权声明**
   `LICENSE` 为专有许可；`index.html` 的 `<head>` 内有版权声明块。

3. **压缩构建**
   `build_minify.py` 用 terser 压缩 JS、cleancss 压缩 CSS、剥离 HTML 注释与 JS 注释。
   115 KB → 97 KB（-16%），且移除了带日期版本号的中文修订注释。

---

## 关于"改成私库"

**不要改。** GitHub Pages 在 Free 账户下只支持公开仓库，改私库会**立即停止发布、网站当场下线**。
私库 + Pages 需要 GitHub Pro（$4/月）。

更关键的：**私库并不能保护源代码。** GitHub Pages 伺服的是构建产物，访客浏览器下载到的 HTML/CSS/JS 一字不少。
私库只挡住仓库文件列表和提交历史，挡不住扒代码。

---

## 若要升级到第 3 层（服务端化）

见 `_protection_review/API_PROTOTYPE_worker.js`（Cloudflare Worker 原型）。
把 ABR/IND/MATRIX 与选型逻辑搬到服务端后，公开仓库只剩空壳表单。

⚠️ 已知局限：对手仍可切换 12 个行业参数反向还原 IND 表。服务端化只能把复制成本
从"点一下"提高到"写个爬虫"，无法归零。是否值得投入，取决于你对这批数据的敏感程度。
