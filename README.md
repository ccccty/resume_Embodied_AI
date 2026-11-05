# 崔天一 · 个人简历站（具身智能 / 边缘感知方向）

本仓库是**具身智能 / 多模态感知 / 边缘部署**方向的个人简历站，纯静态页面，由 GitHub Pages 直接发布。

- 线上地址：<https://ccccty.github.io/resume_Embodied_AI/>
- 英文页：<https://ccccty.github.io/resume_Embodied_AI/en/>
- 打印页（PDF 母版）：<https://ccccty.github.io/resume_Embodied_AI/resume-print.html>
- 在线简历 PDF：<https://ccccty.github.io/resume_Embodied_AI/assets/resume.pdf>

## 目录结构

```
index.html            中文主页面（定位、关于我、经历、项目、教育、技能、开源、荣誉、联系）
en/index.html         英文页面，引用上级 assets/ 目录
resume-print.html     A4 打印母版，assets/resume.pdf 由它导出
assets/resume.pdf     对外投递的 PDF（单页）
assets/css/style.css  样式（唯一外部样式表）
assets/js/main.js     页面交互
assets/img/           头像、获奖照片、站点二维码
一键生成PDF.bat       Windows 下重出 PDF 的脚本（headless Chrome / Edge）
.nojekyll             关闭 Jekyll 处理，保证发布内容与仓库一致
```

页面内的引用全部使用相对路径，因此本仓库改名或被 fork 后，资源不会因为路径写死而失效；
但**二维码里的地址是绝对地址**，仓库改名后必须重新生成，见下节。

## 重出 PDF

方式一（推荐）：双击 `一键生成PDF.bat`，脚本会自动定位 Chrome / Edge，用 `resume-print.html`
以 headless 模式打印为 `assets/resume.pdf`。

方式二（手工）：

```powershell
Remove-Item Env:\ELECTRON_RUN_AS_NODE -ErrorAction SilentlyContinue
& "C:\Program Files\Google\Chrome\Application\chrome.exe" `
  --headless=new --disable-gpu --no-pdf-header-footer `
  --user-data-dir=$env:TEMP\chrome_pdf_emb `
  --print-to-pdf="D:\work\简历\assets\resume.pdf" `
  "file:///D:/work/简历/resume-print.html"
```

导出后自检：页数应为 1 页，PDF 首页右下角二维码应能扫码打开线上站点。

## 更换二维码

`assets/img/qr-site.png` 与 `assets/img/qr-site.svg` 编码的是线上站点地址，目前为
`https://ccccty.github.io/resume_Embodied_AI/`。站点地址变化（改仓库名、绑定自定义域名）时按以下顺序处理：

1. 用 Python 重新生成两张二维码（参数保持 box_size=24、border=4、ERROR_CORRECT_M）：

   ```python
   import qrcode
   from qrcode.image.svg import SvgPathImage

   url = "https://ccccty.github.io/resume_Embodied_AI/"
   img = qrcode.make(url, error_correction=qrcode.constants.ERROR_CORRECT_M,
                     box_size=24, border=4)
   img.save(r"assets/img/qr-site.png")
   svg = qrcode.make(url, image_factory=SvgPathImage, box_size=24, border=4)
   svg.save(r"assets/img/qr-site.svg")
   ```

2. 用 `zxingcpp` 解码两张图片，确认字符串与新地址**完全一致**（不要只看能否扫出内容）。
3. 重新导出 PDF（见上一节），再解码 PDF 首页位图，确认 PDF 内二维码同样是新地址。
4. 提交推送，等 GitHub Pages 构建完成（约 40–90 秒）后，重新下载线上文件解码复核。

## 内容维护约定

- 三个页面（`index.html`、`en/index.html`、`resume-print.html`）是各自独立维护的静态文件，
  改动经历、项目、技能时三处要同步，否则线上页面与实际投递的 PDF 会出现口径不一致。
- 本站定位为**具身智能 / 多模态感知 / 边缘部署**；开源作品区可以放工程作品链接，
  但正文的定位段落（首屏、关于我、教育经历）保持与本站方向一致，不写成其他方向的主线。
- 对外可见文字不出现与本站方向无关的其他简历站地址。

## 独立性说明

本仓库为具身智能方向的独立站点。另有其他方向的简历站由各自仓库维护，两者内容与资源互不引用、
互不复制：本站不链接、不引用其他简历站的任何页面或图片，其他站点亦不引用本站资源。
站点地址只在本仓库的二维码与 README 中出现。
