# 早八惊魂 · 互动短剧

一个纯前端的互动视频网页：主线播到「他做了个决定」时暂停，弹出面板让观众选 A / B，
两条分支各走一遍，再汇合到共同的阳光结尾，结尾后可原地重选另一条。

整个项目没有后端、没有框架、没有任何依赖，把这个文件夹原样上传到 GitHub Pages 就能在线看。

## 目录结构

```
docs/                   ← 就上传这一个文件夹，别拆开
├── index.html          ← 整个网页（CSS / JS 全在里面）
├── poster.jpg          ← 开场背景图（从主线抽的一帧）
├── .nojekyll           ← 空文件，让 GitHub Pages 跳过 Jekyll 构建（别删）
├── qrcode.png          ← 投票二维码（可选，你自己放）
└── videos/
    ├── main.mp4        ← 共同主线 37.9s（起床 → 狂奔 → 同学晕倒 → "他做了个决定"）
    ├── branch_a.mp4    ← 分支A 19.4s（扶人去医务室 → 教室被批评）
    ├── branch_b.mp4    ← 分支B 19.4s（打 120 → 冲去教室赶上签到）
    └── ending.mp4      ← 共同结尾 10.1s（窗边望向阳光）
```

总大小约 35MB，单文件最大 19MB（GitHub 网页上传上限 25MB，能传）。

---

## 上线步骤（只需 4 步）

### 1. 建仓库

打开 https://github.com/new ，Repository name 填 `zaoba-jinghun`（或你喜欢的名字），
选 **Public**，点 **Create repository**。

### 2. 把这个 docs 文件夹整个拖进去

进仓库 → 点 **Add file → Upload files** → 把 **docs 文件夹**（连文件夹一起）拖进那个虚线框。

> 这里就是要"拖整个文件夹"，不用进去挑文件。GitHub 会在仓库里建一个 `docs/` 目录。

拉到下面点 **Commit changes**。

### 3. 设置 Pages —— 目录要选 `/docs`

进仓库 **Settings → Pages**：

- Source：`Deploy from a branch`
- Branch：`main`
- 右边的目录下拉：**选 `/docs`**（默认是 `/ (root)`，一定要改）
- 点 **Save**

> ⚠️ 这一步最容易错。因为网站文件在 `docs/` 里，目录选错就会打开 404。

### 4. 等 1~2 分钟，打开网址

```
https://你的用户名.github.io/zaoba-jinghun/
```

刷新仓库 Settings → Pages 页面，顶部会出现 "Your site is live at ..." 和一串网址，点它就行。
别人点这个网址就能直接看，不用登录、不用装东西。

---

### 如果第 2 步拖完发现没有 docs 文件夹

极少数浏览器会"解包"文件夹、把内容直接铺在仓库根目录。看到根目录直接就是 `index.html` 的话，
第 3 步的目录下拉就选 **`/ (root)`**，其余不变，网址一样。

判断方法：仓库首页看有没有叫 `docs` 的文件夹 —— 有就选 `/docs`，没有就选 `/ (root)`。

---

## 方式二：命令行（以后要改内容时更方便）

```bash
# 在 docs 文件夹里执行
git init
git add .
git commit -m "interactive video: 早八惊魂"
git branch -M main
git remote add origin https://github.com/你的用户名/zaoba-jinghun.git
git push -u origin main
```

用这种方式，文件会直接在仓库根目录，Pages 目录选 `/ (root)`。
以后再改东西，只需要 `git add . && git commit -m "update" && git push`。

---

## 现场怎么操作

| 操作 | 效果 |
|---|---|
| 点「点击开始体验」 | 绕过浏览器自动播放限制 |
| 主线播到剩 2 秒自动暂停 | 画面正好停在「他做了个决定——」这一帧 |
| 点 A / B 卡片，或按键盘 `A` `B` | 立刻切分支，无黑屏 |
| 分支播完 | 自动接共同结尾，不用操作 |
| 结尾播完 → 「重新选择剧情」 | 不刷新页面，回到抉择帧重新选 |
| `F` 键 | 全屏 / 退出全屏（投影用） |

手机竖屏打开会提示「横过来看」，横屏体验和电脑一致。

---

## 想改东西，改这几处

1. **弹面板的时机**：`index.html` 里搜 `triggerBeforeEnd`（单位秒，默认 `2.0`）。
   主线 37.9s，设 2.0 就停在 35.9s。调大停得早，调小更贴结尾。
2. **选项文案**：搜 `立马扶起同学` / `拨打 120`，改 `<span class="t1">`（主文案）和 `<span class="t2">`（副文案）。
3. **标题**：搜 `早八惊魂` —— 开场大标题、`<title>` 各一处。
4. **二维码**：把你的投票二维码存成 `qrcode.png` 放在 `index.html` 同级，刷新后右侧自动出现「现场扫码投票」栏；没有这个文件就自动收成单栏，不会报错。
5. **换视频**：直接替换 `videos/` 里四个文件，文件名保持不变即可。

---

## 几个可能踩的坑

- **双击 `index.html` 打不开视频**：用 Chrome / Edge 一般没问题；如果不行，在文件夹里跑 `python -m http.server 8000`，然后开 `http://localhost:8000`。
- **上线后视频转圈不动**：先确认 `videos` 文件夹在仓库根目录（不是在某个子文件夹里），文件名大小写要和 `index.html` 里写的一致。
- **改了代码但网址没变**：GitHub Pages 有缓存，等 1~2 分钟，或按 `Ctrl+F5` 强制刷新。
- **本地改了想重新上传**：网页上传的方式要重新拖一次覆盖；用命令行的话 `git add . && git commit -m "update" && git push` 即可。

## 无缝切换是怎么做的

四个 `<video>` 标签同时存在，全部 `preload="auto"`；进入页面前还会把每条视频 seek 到 0.05s 再回 0，
强制浏览器解码出首帧（`warmUp()`）。切换时是「先 `play()` 成功 → 再把这一层的 `opacity` 置 1 并抬到最高 `z-index`」，
永远是新画面盖住旧画面，所以既不会闪一下黑帧，也没有两层叠在一起的重影。
