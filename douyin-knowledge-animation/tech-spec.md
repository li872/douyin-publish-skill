# 技术骨架与关键代码模式

单文件 HTML，无框架，'use strict' ES5 风格。参考成品：`红绿灯配时_抖音竖屏版.html`（红绿灯项目）。

## 舞台结构

```html
<div id="stage">                    <!-- 720×1280 固定设计稿 -->
  <section class="slide cover">…</section>   <!-- 封面：canvas 全屏出血 + 居中标题 -->
  <section class="slide">                    <!-- 内容页 ×9 -->
    <div class="hd">
      <div class="stepk"><b>01</b>小标题</div>
      <div class="punch">金句大字<br>关键词<i>绿色强调</i></div>
    </div>
    <div class="cwrap"><canvas></canvas></div>
  </section>
  <div class="wm">水印（bottom:178px，避开文案遮挡）</div>
</div>
```

## 核心 CSS 数值（已验证的定稿值）

```css
#stage{ position:fixed;left:50%;top:50%;width:720px;height:1280px;
  transform:translate(-50%,-50%) scale(1); }         /* JS 按视口算 scaleK */
.slide{ padding:0 0 215px }                          /* 底部安全区 */
.hd{ padding:100px 48px 18px; text-align:center }    /* 顶部安全区 */
.cwrap{ flex:1; margin:8px 44px 0 }                  /* 两侧安全区 */
.stepk{ font-size:21px; font-weight:700; color:cyan系; text-shadow 光晕 }
.punch{ font-size:46px; font-weight:900; line-height:1.4 }
/* 文字色全用纯白 #ffffff；强调色 --grn:#3ce97f --red:#ff5061 --amb:#ffb340 --cyn:#4fd8ff */
```

## 关键 JS 模式

```js
// 1) 等比缩放：scaleK = min(innerW/720, innerH/1280)，resize 时重算并 fitCanvas(cur)

// 2) canvas 清晰度：d = devicePixelRatio * scaleK
//    cv.width = wCss*d; ctx.setTransform(d,0,0,d,0,0); 绘制仍用 CSS 像素坐标

// 3) 字体函数全局放大（所有 canvas 文字唯一入口）
function F(px,bold){ px=Math.max(16,Math.round((px||12)*1.3));
  return (bold?'600 ':'')+px+'px '+MONO; }

// 4) 主循环：每页独立计时 + SPD 提速数组（卡一句歌词）
var SPD=[1.5, 2, 2.2, ...];             // 让每页高潮画面 ~2.5s 内出现
var t=(now-stepStart)/1000*SPD[cur];
if(ended) t=endT;                        // 末页定格：时间冻结
try{ DRAW[cur](g,w,h,t); }catch(e){}     // 单帧异常不中断

// 5) 自动放映与定格
if(auto && el>=lim){                     // lim = 封面3000ms / pageSec*1000
  if(cur>=N-1){ endT=t; ended=true; setAuto(false); }  // 播完锁帧
  else go(cur+1);
}

// 6) 交互：click / Enter → go(0)+setAuto(true)；箭头翻页先 setAuto(false)
//    空格翻页必须 e.preventDefault()（防按钮焦点冲突）
```

## 绘制函数约定

- 签名统一 `function drawN(g,w,h,t)`，全部用 w/h 相对坐标，安全区改动后自动适应
- 动画节奏：`ease((t-延迟)/时长)` 分段推进；出现顺序 = 讲解顺序
- 数据图表：内部再留 8% 边距（x0=w*0.08）；刻度/轴题纯白
- 状态标注模式：数值旁边跟人话标签（红"排队爆了！"/黄"绿灯空转"/绿"✓ 刚刚好"）
- 封面 canvas 全屏出血做氛围，但灯/车流等元素放 h*0.155~0.765 之间（避遮挡）

## 常见坑

- 竖屏窄幅：横向布局（环+侧边数据）要改为上下堆叠（环在上 30%，数据列在下）
- 中文在 monospace 字体里比拉丁字宽，measureText 实测别按字数估
- 行高：punch 两行 46px 时 .hd 总高 ~300px，cwrap 剩 ~760px，纵向图表按此设计
