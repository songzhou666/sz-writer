# HTML 格式化交付模板

> 用途：S7 审核通过后，将终稿渲染为带完整格式的 HTML 文档交付用户。HTML 可在浏览器中直接打开、打印为 PDF，或复制到 Word 保留字体/颜色/排版。
> 触发：用户要求"文档格式"、"红头文件"、"Word格式"、"带格式"，或默认正式公文交付时启用。

## 一、HTML 输出规范

### 1.1 字体映射

| 国标字体 | CSS font-family | 说明 |
| --- | --- | --- |
| 方正小标宋简体 | "FZXiaoBiaoSong-B05S", "方正小标宋简体", serif | 标题、发文机关标志 |
| 仿宋_GB2312 | "FangSong_GB2312", "仿宋_GB2312", "FangSong", serif | 正文、主送、落款 |
| 黑体 | "SimHei", "黑体", sans-serif | 一级标题、密级 |
| 楷体_GB2312 | "KaiTi_GB2312", "楷体_GB2312", "KaiTi", serif | 二级标题、签发人姓名 |

### 1.2 字号映射

| 国标字号 | CSS font-size |
| --- | --- |
| 初号 | 42pt |
| 小初 | 36pt |
| 1号 | 26pt |
| 小1 | 24pt |
| 2号 | 22pt |
| 小2 | 18pt |
| 3号 | 16pt |
| 4号 | 14pt |

## 二、完整 HTML 模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<title>{公文标题}</title>
<style>
  @page {
    size: A4;
    margin: 37mm 26mm 35mm 28mm;
  }
  /* 页码说明：GB/T 9704-2012 要求页码为 4号半角宋体阿拉伯数字、单页码居右空1字、双页码居左空1字。
     浏览器（Chrome/Edge/Safari）均不支持 CSS Paged Media 的页码盒（@page 内的 @bottom-*），
     写了也不生效，故此处不生成页码；如需公文页码，请在 Word 中插入页码后再打印。 */
  body {
    font-family: "FangSong_GB2312", "仿宋_GB2312", "FangSong", serif;
    font-size: 16pt;
    line-height: 29pt;
    color: #000;
  }
  /* 版头 */
  .banhead {
    text-align: center;
    /* 发文机关标志下空二行 */
    margin-bottom: 2em;
  }
  .org-name {
    font-family: "FZXiaoBiaoSong-B05S", "方正小标宋简体", serif;
    font-size: 42pt;
    color: #C00000;
    letter-spacing: 4px;
  }
  /* 发文字号行（下行/平行文：居中） */
  .doc-head-center {
    text-align: center;
    margin-bottom: 4mm;
  }
  /* 发文字号行（上行文：发文字号居左、签发人居右，同行） */
  .doc-head-between {
    display: flex;
    justify-content: space-between;
    margin-bottom: 4mm;
  }
  .doc-number {
    font-size: 16pt;
  }
  .signer {
    font-size: 16pt;
    text-align: right;
  }
  /* 红色分隔线：印在发文字号（及签发人）之下 4mm 处，与版心等宽（GB/T 9704-2012 §7.2.7） */
  .red-line {
    border: none;
    border-top: 0.35mm solid #C00000;
    margin: 0 0 2em 0;
  }
  .signer-name {
    font-family: "KaiTi_GB2312", "楷体_GB2312", "KaiTi", serif;
  }
  .secret-level {
    font-family: "SimHei", "黑体", sans-serif;
    font-size: 16pt;
    text-align: left;
  }
  /* 主体 */
  .title {
    font-family: "FZXiaoBiaoSong-B05S", "方正小标宋简体", serif;
    font-size: 22pt;
    text-align: center;
    margin: 1em 0;
  }
  .recipient {
    font-size: 16pt;
    text-align: left;
  }
  .content p {
    text-indent: 2em;
    margin: 0;
  }
  .h1 {
    font-family: "SimHei", "黑体", sans-serif;
    font-size: 16pt;
    text-indent: 2em;
  }
  .h2 {
    font-family: "KaiTi_GB2312", "楷体_GB2312", "KaiTi", serif;
    font-size: 16pt;
    text-indent: 2em;
  }
  .h3 {
    font-family: "FangSong_GB2312", "仿宋_GB2312", "FangSong", serif;
    font-weight: bold;
    font-size: 16pt;
    text-indent: 2em;
  }
  .attachment {
    font-size: 16pt;
    text-indent: 2em;
    margin-top: 1em;
  }
  .signature {
    font-size: 16pt;
    text-align: center;
    margin-top: 3em;
  }
  .date {
    font-size: 16pt;
    text-align: right;
  }
  .note {
    font-size: 16pt;
    text-align: left;
    margin-top: 1em;
  }
  /* 版记 */
  .banji {
    margin-top: 4em;
    font-size: 14pt;
    border-top: 1px solid #000;
  }
  .cc {
    text-indent: 1em;
  }
  .print-info {
    display: flex;
    justify-content: space-between;
  }
</style>
</head>
<body>

<!-- 版头（红头文件） -->
<!-- 如需红头文件，保留以下版头部分；事务文书可删除版头 -->
<!-- 版头要素自上而下：发文机关标志 → 发文字号（上行文含签发人） → 红色分隔线（GB/T 9704-2012 §7.2） -->
<div class="banhead">
  <div class="org-name">{发文机关全称}文件</div>
</div>
<!-- 下行/平行文：发文字号居中 -->
<div class="doc-head-center">
  <span class="doc-number">{发文机关代字}〔{年份}〕{序号}号</span>
</div>
<!-- 上行文：用下面这块替换上一块（发文字号居左、签发人居右，二者同行）
<div class="doc-head-between">
  <span class="doc-number">{发文机关代字}〔{年份}〕{序号}号</span>
  <span class="signer">签发人：<span class="signer-name">{签发人姓名}</span></span>
</div>
-->
<hr class="red-line">

<!-- 密级紧急程度（如需）
<div class="secret-level">{密级}　{紧急程度}</div>
-->

<!-- 主体 -->
<h1 class="title">{公文标题}</h1>
<div class="recipient">{主送机关}：</div>
<div class="content">
  <p>{正文第一段}</p>
  <p class="h1">一、{一级标题}</p>
  <p>{内容}</p>
  <p class="h2">（一）{二级标题}</p>
  <p>{内容}</p>
  <p class="h3">1. {三级标题}</p>
  <p>{内容}</p>
</div>
<div class="attachment">附件：{附件名称}</div>
<div class="signature">{发文机关署名}</div>
<div class="date">{成文日期}</div>
<div class="note">（联系人：{联系人}；联系电话：{联系电话}）</div>

<!-- 版记 -->
<div class="banji">
  <div class="cc">抄送：{抄送机关}</div>
  <div class="print-info">
    <span>{印发机关}</span>
    <span>{印发日期}</span>
  </div>
</div>

</body>
</html>
```

## 三、使用规则

1. **默认正式公文**：法定公文（通知/请示/报告/函等）默认带版头（红头文件）输出 HTML。
2. **事务文书**：总结/计划/简报/讲话稿等不带版头，仅输出主体部分 HTML。
3. **用户明确要求"红头文件"**：启用版头，发文机关标志红色 + 红色分隔线。
4. **用户明确要求"普通格式"**：不输出版头。
5. **占位符替换**：用 `{}` 包裹的占位符需替换为用户实际内容；无对应内容的整段删除。
6. **输出方式**：HTML 代码用代码块包裹交付，用户复制保存为 `.html` 文件即可用浏览器打开。
7. **打印PDF**：浏览器打开后 Ctrl+P 打印，选择"另存为PDF"，页边距已设为标准值。
8. **页码限制**：浏览器不支持自动生成国标公文页码（4号宋体、单页居右/双页居左），HTML 版默认不含页码；如需页码请在 Word 中设置后再打印。

## 四、双轨交付

交付时同时提供：
- **纯文本版**：Markdown 格式，方便快速复制文字内容。
- **HTML格式化版**：带完整字体/颜色/排版，用于正式打印或导出PDF。

交付摘要注明："已同时提供纯文本版和HTML格式化版，HTML版可直接保存为.html用浏览器打开打印。"
