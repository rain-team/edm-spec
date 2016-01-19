# edm-spec

> EDM 制作规范

## html

只使用 `<div>` `<a>` `<span>` `<table>` `<tr>` `<td>` `<br>`  `<map>` `<area>` 标签可以避免很多默认样式造成的错位问题

## CSS书写方式

请在行间书写 CSS
```html
<div style="height: 69px;"></div>
```

如不考虑outlook客服端。可以把CSS写在 `<style>` 标签内
```css
<style>
  .box{ height: 69px;}
</style>
<div class="box">text</div>
```

## 禁止使用的 CSS
### 中文字体
不要使用类似 `font-family:微软雅黑;` 的中文字体样式，建议使用 `Microsoft Yahei`。因为中文字体容易被解析成 `style="font-family:"微软雅黑""` 出现2对 `"` 导致整条样式失效。

### float position
不要使用 `float:left; position:absolute;` 会因为邮箱兼容性导致错位

### 使用 table 代替 float position

使用简单的嵌套 `<table>`，提高邮件在各大客户端的兼容性。
```html
<table border="0"  cellpadding="0"  cellspacing="0">
    <tr>
      <td style="text-align:left;">左侧</td>
      <td style="text-align:right;">右侧</td>
    </tr>
</table>
```
**务必在 `<table>` 中加上 `border="0"  cellpadding="0"  cellspacing="0"`**

## 不要使用跨行跨列
尽量不要使用 table 跨行跨列：
```html
<td rowspan="2" colspan="2">
```
因为跨行跨列后代码会变得很难维护而且部分邮箱不兼容，尽量使用多个 table 嵌套实现功能。

比如要实现一个 1:1:2 的布局：

```html
<table border="0"  cellpadding="0"  cellspacing="0">
    <tr>
      <td>
         <table border="0"  cellpadding="0"  cellspacing="0">
            <tr>
              <td>1</td>
              <td>1</td>
            </tr>
         </table>
      </td>
      <td> 2 </td>
    </tr>
</table>
```

## 给 td 设置宽度

建议在每个单元格内设置宽度
```html
<td width="200"></td>
```
width height 属性中不要设置 `px` ，会在 outlook 失效
```html
<td width="200px"></td>    错误
```

## 不要使用 form表单 JavaScript Flash
邮箱不完全不支持JavaScript Flash，部分邮箱支持 form表单

## 链接必须全部是绝对路径

```html
// 可以
<a href="http://www.emailcar.net/index.html">
// 不可以
<a href="index.html">
```

## 退订投诉链接

模板加入预览退订投诉信息，加在头部或底部（根据设计稿），如果没有设计进去，默认加在头部。

**此链接只适用于 http://www.emailcar.net**

```html
&lt;%=unsubscribe%&gt;   退订变量
&lt;%=complaints%&gt;    投诉变量
&lt;%=tplview%&gt;       预览变量
```

## 插入视频

需要插入视频，用下面这种方式兼容性比较好，需要考虑的是没有显示视频设置一个背景图片（163，QQ，sina等主流网页邮箱支持）

```html
<video width="320" height="240" controls autoplay>
  <source src="http://www.w3school.com.cn/i/movie.ogg" type="video/ogg" />
  <source src="http://www.w3school.com.cn/i/movie.mp4" type="video/mp4" />
  <source src="http://www.w3school.com.cn/i/movie.webm" type="video/webm" />
  <object data="http://www.w3school.com.cn/i/movie.mp4" width="320" height="240">
    <embed width="320" height="240" src="http://www.w3school.com.cn/i/movie.swf" />
  </object>
</video>
```

## 图片注意点
1. 建议不使用背景图片，部分客户端是不显示图片，如果使用则要同时添加背景颜色。
2. 图片属性添加display:block，因为在图像下填充额外的像素
3. 图片属性添加border:0;，因为加链接会产生蓝色边框
4. 建议图片都设置固定的宽和高
5. 图片真实尺寸应该与设置 的 `width` `height` 一致
6. 图片储存尽可能小，JPG(质量为50-80)。GIF PNG8(质量128)
  颜色比较丰富的图片用JPG格式，图会清晰点 文件也小点
  颜色比较单一的用GIF PNG8，图会清晰点 文件也小点



## Outlook注意点
1. padding需要写在<td>上，写在其它地方无效。
2. 背景图片也不兼容。outlook支持纯色
3. css必须在行间，在头部不支持
4. outlokk有默认的line-height，自己设置line-height无效（在切图的时候要注意多预留空隙）


## Outlook建议写法
1. 建议
  - 给`<tr>、<td>`加背景颜色（如果有背景图片，需截取和背景色相仿的颜色做背景色）
    不提倡用：`<td background="#f4f4f4">`
    建议用法：`<td  style="background-color:#f0f0f0;">`
  - 给文字模块的<table>加属性`cursor: default;`
  - 由于outlook自带line-height,会造成文字高度溢出。
    对大段文字用div了拆分每一行，用div的height来控制行高，不依赖自动换行，更加精确。
    例子（经典的三格布局）：
``` css
<tr>
		<td>
			<table>
				<tbody>
					<tr>
						<td>
							<img src="" style="display:block; padding:0px; border:0px; margin:0px;" />
						</td>
				        <td style="background-color:#f4f4f4;">
							<div style="width:608px;height:84px;color:#2c2c2c;font-family:Microsoft Yahei; font-size:14px;line-height: 28px;padding:0; margin:0; border:0px;">
								<div style="width:608px;height:28px;padding:0; margin:0; border:0px;">第一行文字</div>
								<div style="width:608px;height:28px;padding:0; margin:0; border:0px;">第二行文字</div>
								<div style="width:608px;height:28px;padding:0; margin:0; border:0px;">第三行文字</div>
							</div>
						</td>
						<td>
							<img src="" style="display:block; padding:0px; border:0px; margin:0px;" />
						</td>
					</tr>
				</tbody>
			</table>
		</td>
	</tr>
```