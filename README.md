# metalabrasive-selection-AI

🌐 **Live site:** [https://hannibalzhang007.github.io/metalabrasive-selection-AI/](https://hannibalzhang007.github.io/metalabrasive-selection-AI/)

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

## 日常改代码的正确流程（2026-11-09 更新）

> ⚠️ **实际惯例已变更**：仓库自 2026-10-19 起，所有迭代（FAQ、深度 guides、双轨询盘板块、GEO 改造等）都是**直接 patch 发布版 `index.html`** 完成的（仓库内 `patch_*.py` 即为此用）。`_protection_review/build_minify.py` 是早期压缩构建脚本，**直接跑会丢 10-19 之后新增的 FAQ / guides / 双轨板块**，请勿盲目执行。

### 当前推荐流程（直接编辑发布版）
```bash
# 1. 改前备份发布版
cp index.html index.html.bak

# 2. 直接编辑发布版（脚本 patch 或精确定位编辑；已压缩，注意 CRLF）
编辑 index.html

# 3. 本地自检：JSON-LD 可解析 / 标签配平 / 无占位符 / 锚点齐全
#    （校验思路见历史 patch_*.py）

# 4. 仅推送公开文件（勿推送 _protection_review/ 与内部运营文档 *.md）
git add index.html llms.txt sitemap.xml README.md
git commit -m "Update"
git push
```

### 如需重新走压缩构建（谨慎，仅当 _protection_review/ 已含全部新增内容）
```bash
cd _protection_review && python build_minify.py
cp ..\dist\index.html ..\index.html
```
否则请沿用上面的「直接编辑发布版」流程。

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
