# vue-pdf-app
基于Vue+PDF.js实现的手机版预览

# Vue移动端在线预览之PDF实现

![设计坞-20200313-4720547](http://image.damoxueyuan.com/uPic/设计坞-20200313-4720547.jpg)

## 效果演示
![image-20200313201359741](http://image.damoxueyuan.com/uPic/image-20200313201359741.png)




## 一、搞实验

> 花了整整一天半时间调研和实验

### 实验一【vue-pdf】： 

https://github.com/FranckFreiburger/vue-pdf

```vue
<template>
<pdf src="./static/relativity.pdf"></pdf>
</template>

<script>
import pdf from 'vue-pdf'

export default {
components: {
pdf
}
}
```



### 实验二【preview服务器】：

[http://preview.damotimes.com/onlinePreview?url=http://image.damoxueyuan.com%2Ffiles%2F7%25E3%2580%2581%25E6%258B%2585%25E4%25BF%259D%25E6%2596%25B9%25E8%25B5%2584%25E6%2596%2599.pdf](http://preview.damotimes.com/onlinePreview?url=http://image.damoxueyuan.com%2Ffiles%2F7%E3%80%81%E6%8B%85%E4%BF%9D%E6%96%B9%E8%B5%84%E6%96%99.pdf)



### 实验三【create-react-app】

> pdf.js源码 https://github.com/mozilla/pdf.js/



### 实验四【PDF.js + Vue】

> 达到我想要，可以综合出更好的版本



## 二、首选实验四综合记录

PDF.js官网：https://github.com/mozilla/pdf.js/

VUE.js DEMO来源：https://juejin.im/post/5d2feeeff265da1b971aaac4

> 实验：android手机，微信浏览器，h5浏览器都跑得不错



### 解释



#### pdfjs是pdf的渲染器

#### web worker

> 为了提升解析和渲染PDF的性能，pdf.js引入了[Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)，不了解web worker的童鞋也可以戳阮一峰老师的[这篇文章](http://www.ruanyifeng.com/blog/2018/07/web-worker.html)

PDFJS.GlobalWorkerOptions.workerSrc 为了提升解析和渲染PDF的性能

拿到PDFDocumentLoadingTask之后，根据pdf页码获取当前页的pdf

设置PDf文档的页面尺寸(展示比例)

### 描述过程

> PDFJs通过`canvas`将pdf内容渲染到浏览器，会在页面定义canvas。
>
> 首先调用 `PDFJS.getDocument` 方法获取一个[PDFDocumentLoadingTask](https://mozilla.github.io/pdf.js/api/draft/PDFDocumentLoadingTask.html)。
>
> 拿到`PDFDocumentLoadingTask`之后，根据pdf页码获取当前页的pdf。
>
> 根据当前页，设置PDf文档的页面尺寸(展示比例)。
>
> 准备把pdf内容渲染canvas中
>
> 扩展:文本复制



### 过程对应代码

#### 定义canvas

> PDFJs通过`canvas`将pdf内容渲染到浏览器，会在页面定义canvas。
>
> >```html
> ><canvas id="the-canvas" style="border:1px  solid black"></canvas>
> >```

#### 获取pdf对象

>首先调用 `PDFJS.getDocument` 方法获取一个[PDFDocumentLoadingTask](https://mozilla.github.io/pdf.js/api/draft/PDFDocumentLoadingTask.html)。
>
>>```js
>>let pdf = await PDFJS.getDocument(url)
>>```

#### 读取当前页

> 拿到`PDFDocumentLoadingTask`之后，根据pdf页码获取当前页的pdf
>
> >```JS
> >let page = await pdf.getPage(num) // num 为页码，如 1
> >```

#### 设置当前页展示比例

>为当前页，设置PDf文档的页面尺寸(展示比例)
>
>>```js
>>let scale = 1.5;
>>let viewport = page.getViewport(scale);
>>```

#### pdf->canvas

>准备把pdf内容渲染canvas中
>
>>```js
>>let renderContext = {
>>    canvasContext: context, // 此为canvas的context
>>    viewport: viewport
>>};
>>await page.render(renderContext); // 这里await是为了后面渲染pdf文本
>>```

#### 扩展：文本复制

> 最后，拿到pdf的内容渲染成文本
>
> >```js
> >let textContent = await page.getTextContent()
> >/* ... */
> >// 创建新的TextLayerBuilder实例
> >var textLayer = new TextLayerBuilder({
> >    textLayerDiv: textLayerDiv, // 放置文本的dom
> >    pageIndex: page.pageIndex, // pdf页码
> >    viewport: viewport
> >});
> >
> >textLayer.setTextContent(textContent);
> >
> >textLayer.render();
> >```

### 使用步骤

#### 1. 安装PDF.js

```shell
npm i pdfjs-dist
```

or

```shell
yarn add pdfjs-dist
```



#### 2. 在vue中使用

```vue
import PDFJS from 'pdfjs-dist';
PDFJS.GlobalWorkerOptions.workerSrc = 'pdfjs-dist/build/pdf.worker.js';

```







## 三、完整代码

### 我的代码

```vue
<template>
<div>

<div id="viewerContainer" ref="container" @scroll="orderScroll">
<div id="viewer" class="pdfViewer">
<!-- <div class="page" data-page-number="1" data-loaded="true" style="width: 695px; height: 774px;" >
<div class="canvasWrapper" style="width: 695px; height: 774px;">
<canvas width="1992" height="2214" aria-label="Page 1" style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);" ></canvas>
</div>
</div> -->
<div class="page" v-for="item in numPages" :key="item" :style="zoomArr[zoom]">
<div class="canvasWrapper"   :style="zoomArr[zoom]"></div>
<div class="loadingIcon"></div>
</div>
</div>
</div>

<footer class="footer">
<div class="footer-wrapper">
<button class="toolbarButton pageUp" @click="pageUp"></button>
<button class="toolbarButton pageDown" @click="pageNext"></button>
<div class="pageNumber" @click="calcOffset">{{currentPageNum+1}}/{{numPages}}</div>
<button class="toolbarButton zoomOut" @click="scaleMinus"></button>
<button class="toolbarButton zoomIn"  @click="scalePlus"></button>
</div>
</footer>

<!-- <div class="loading-wait">{{loadProccess}}</div> -->
<div class="loading-wait" v-if="!pdf">
<div class="loading-wait-mask"></div>
<div class="loading-content">
<div class="loading-gif">
<img src="./images/loading.gif" style="width:50px;height:50px">
</div>
<div>进度：{{loadProccess?loadProccess:'20'}}%</div>
<div class="text">
加载中，请耐心等待...
</div>
</div>
</div>
</div>
</template>

<script>
/* eslint-disable */
import PDFJS from "pdfjs-dist";
import { TextLayerBuilder } from "pdfjs-dist/web/pdf_viewer";
// import "pdfjs-dist/web/pdf_viewer.css";
PDFJS.GlobalWorkerOptions.workerSrc = "pdfjs-dist/build/pdf.worker.js";
let zoomConfig = [
{width: '248px', height: '271px'}, 
{width: '298px', height: '325px'},
{width: '397px', height: '442px'},
{width: '496px', height: '553px'},
{width: '596px', height: '664px'},
{width: '695px', height: '774px'},
{width: '794px', height: '885px'},
{width: '894px', height: '975px'},
{width: '993px', height: '1084px'},
{width: '1092px', height: '1192px'},
{width: '1291px', height: '1409px'},
{width: '1490px', height: '1626px'},
{width: '1688px', height: '1842px'},
{width: '1887px', height: '2102px'}
]
/* eslint-disable */ 
export default {
name: "HelloWorld",
props: {
msg: String
},
data(){
return {
url: 'http://image.damoxueyuan.com/files/4%E3%80%81%E6%89%BF%E8%AF%BA%E5%87%BD.pdf',
// 第二个：效果特别好
// url: 'http://image.damoxueyuan.com/files/6%E3%80%81%E5%8F%91%E8%A1%8C%E6%96%B9%E8%B5%84%E6%96%99.pdf',
// url: 'http://image.damoxueyuan.com/file%E6%80%8E%E4%B9%88%E6%8A%8APDF%E8%BD%AC%E6%88%90%E5%9B%BE%E7%89%87%EF%BC%9FPDF%E8%BD%AC%E5%9B%BE%E7%89%87%E6%9C%89%E5%93%AA%E4%BA%9B%E5%B0%8F%E6%8A%80%E5%B7%A7%EF%BC%9F.pdf',
// http://image.damoxueyuan.com/files/7%E3%80%81%E6%8B%85%E4%BF%9D%E6%96%B9%E8%B5%84%E6%96%99.pdf,
container:null,
scaleObj:{},
numPages:0,
scale:1.5,
canvasList:[],
zoomArr: zoomConfig,
zoom:1,
pageArr:[],
canvasWidth: 375,
currentPageNum:0,
minWidth:250,
lastOffsetTop:0,//距顶上次一次
currentOffsetTop:0,//距顶这一次
pdf: null,
loadProccess: 0,
}
},
mounted() {
this.$nextTick(() => {

// this.initPDF();
this.initPDFByNew();
});
},
methods: {
pageUp(){
let currentPageNum = this.currentPageNum -1 ;
if(currentPageNum < 0){
return;
}

let pages = this.pageArr.length ? this.pageArr : this.$refs.container.querySelectorAll('.page');
let toElement = pages[currentPageNum];
toElement.scrollIntoView({
block: 'center',
behavior: 'smooth'
});
// this.currentPageNum = currentPageNum;
},
pageNext(){
let currentPageNum = this.currentPageNum+1;
if(currentPageNum === this.numPages){
return;
}
let pages = this.pageArr.length ? this.pageArr : this.$refs.container.querySelectorAll('.page');

// let pages[currentPageNum];
let toElement = pages[currentPageNum];
toElement.scrollIntoView({
block: 'center',
behavior: 'smooth'
});
// this.currentPageNum = currentPageNum;

},
getFirstOffset(){ // 第一个元素该移动的距离
//当前页高度
let zoomHeight = +this.zoomArr[this.zoom]['height'].slice(0,-2);
// 可视高度
let clientHeight = this.$refs.container.clientHeight ;
// 第一个元素该移动的距离
return (clientHeight - zoomHeight - 11)/2;
},
isLastPage(){

},
calcOffset(){
let a = this.$refs.container.scrollHeight
let b = this.$refs.container.clientHeight
let c = this.$refs.container.scrollTop
//当前页高度
let zoomHeight = +this.zoomArr[this.zoom]['height'].slice(0,-2);

console.log('滚动条',a)
console.log('可视区',b)
console.log('PDF高度',zoomHeight);

console.log('距离顶部',c);

console.log('变化结果师', this.currentOffsetTop - this.lastOffsetTop);
console.log("======================结束");

this.lastOffsetTop = this.currentOffsetTop;

},
scrollToPage(){
// 滚动条总高度/总页数 = 每页高度
// 距离顶部/每页高度 = 当前页  关键


// 获得第一个该移动的位置
let firstOffset = this.getFirstOffset();
// 最后一页和第一页，作特殊处理
// 距顶部高度
let offsetTop = this.$refs.container.scrollTop + firstOffset;
//当前页高度
let zoomHeight = +this.zoomArr[this.zoom]['height'].slice(0,-2);
this.currentPageNum = Math.round(offsetTop/(zoomHeight+11));

},
orderScroll(e){
this.scrollToPage();
},
initPDFByNew(){
let loadingTask = PDFJS.getDocument(this.url);
loadingTask.onProgress = (progress) => {
var percent = parseInt(progress.loaded / progress.total * 100);
// $('#' + transfer_id).css('width', percent + '%');
this.loadProccess = percent;
}
loadingTask.promise.then((pdf) => {
this.pdf = pdf;
// load pdf
this.numPages =  this.pdf.numPages;
this.container = this.container || document.querySelector('#container');
this.handlePDFPage();
});
},
async initPDF(){
//  读取PDF文档
this.pdf =       await this.getPDF(this.url);
this.numPages =  this.pdf.numPages;
this.container = this.container || document.querySelector('#container');
this.handlePDFPage();
},



refreshShow(){
let currentZoom = this.zoomArr[this.zoom];
this.canvasList.map((canvas)=>{
canvas.style.width = currentZoom.width;
canvas.style.height = currentZoom.height;
})
},

scaleMinus(){
if(this.zoom === 0) return;
this.zoom--;
this.refreshShow();
},
scalePlus(){
if(this.zoom === this.zoomArr.length - 1) return;
this.zoom++;
this.refreshShow();
},

async handlePDFPage(){//开始处理PDF循环,异步渲染
console.log(this.numPages);

// this.numPages -1
for(let i = 0; i <= this.numPages; i++) {
try{
await this.rendPDF(this.pdf, i);
// setTimeout(() => {
// this.rendPDF(this.pdf, i);
// }, 0);
} catch(e) {
// console.error(e)
}
}
},

async getPDF(url) {  // 读取pdf文档
let pdf = await PDFJS.getDocument(url);
return pdf;
},

createWrapperDom(page){
// <div class="page" data-page-number="1" data-loaded="true" style="width: 695px; height: 774px;" >
//   <div class="canvasWrapper" style="width: 695px; height: 774px;">
//     <canvas width="1992" height="2214" aria-label="Page 1" style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);" ></canvas>
//   </div>
// </div>
let pageDiv = document.createElement('div');
pageDiv.setAttribute('id', 'page-' + (page.pageIndex + 1));
pageDiv.setAttribute('data-page-number', page.pageIndex + 1);
pageDiv.setAttribute('data-loaded', true);

let canvasWrapper = document.createElement('div');
pageDiv.appendChild(canvasWrapper);



},



async rendPDF(pdf, num) {

// debugger;
// console.log(num);

let page = await pdf.getPage(num)
// 设置展示比例
let scale = 1.5;
let viewport = page.getViewport(scale);
// viewport.height = 1998;
// viewport.width = 2219;

// let pageDiv = document.createElement('div');
// pageDiv.setAttribute('id', 'page-' + (page.pageIndex + 1));
// pageDiv.setAttribute('style', 'position: relative');
let pageDiv = document.createElement('div');
pageDiv.setAttribute('id', 'page-' + (page.pageIndex + 1));
pageDiv.setAttribute('data-page-number', page.pageIndex + 1);
pageDiv.setAttribute('data-loaded', true);

// let canvasWrapper = document.createElement('div');
//     pageDiv.setAttribute('class', 'canvasWrapper');
//     pageDiv.appendChild(canvasWrapper);

//     document.querySelector('#viewer').appendChild(canvasWrapper);

// console.log(1111,this.$refs['pageIndex'+num]);
let pageArr = this.pageArr.length ? this.pageArr : this.$refs.container.querySelectorAll('.page');
let canvasWrapper =  pageArr[num-1].querySelector('.canvasWrapper');
let loadingIcon  =   pageArr[num-1].querySelector('.loadingIcon');
loadingIcon.style.display = 'none';

this.pageArr = pageArr;

//  debugger;
let canvas = document.createElement('canvas');
canvasWrapper.appendChild(canvas);
let context = canvas.getContext('2d');
canvas.height = viewport.height;
canvas.width = viewport.width;
// console.log(viewport);


this.canvasList.push(canvas);

let renderContext = {
canvasContext: context,
viewport: viewport
};
// console.log('1111222333',num,canvas);


await page.render(renderContext).promise;
// // debugger
// let textContent = await page.getTextContent()
// // 创建文本图层div
// const textLayerDiv = document.createElement('div');
// textLayerDiv.setAttribute('class', 'textLayer');
// textLayerDiv.setAttribute('style', `width: ${viewport.width}px; margin: 0 auto;`)
// // 将文本图层div添加至每页pdf的div中
// pageDiv.appendChild(textLayerDiv);

// // 创建新的TextLayerBuilder实例
// var textLayer = new TextLayerBuilder({
//     textLayerDiv: textLayerDiv,
//     pageIndex: page.pageIndex,
//     viewport: viewport
// });
// textLayer.setTextContent(textContent);
// textLayer.render();
// canvas.style.width = this.canvasWidth + 'px';
this.refreshShow();

}
}
};
</script>

<style scoped>
/* Copyright 2016 Mozilla Foundation
*
* Licensed under the Apache License, Version 2.0 (the "License");
* you may not use this file except in compliance with the License.
* You may obtain a copy of the License at
*
*     http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed to in writing, software
* distributed under the License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
* See the License for the specific language governing permissions and
* limitations under the License.
*/

* {
padding: 0;
margin: 0;
}

html {
height: 100%;
width: 100%;
overflow: hidden;
font-size: 10px;
}
body {
background: url(images/document_bg.png);
color: rgba(255, 255, 255, 1);
font-family: sans-serif;
font-size: 10px;
height: 100%;
width: 100%;
overflow: hidden;
}

/* 翻页页面 */
.loading-wait{
position: fixed;
top:0;
left:0;
right:0;
bottom:0;
z-index: 99;
}
.loading-wait-mask{
position: absolute;
top:0;
left:0;
right:0;
bottom:0;
background-color: #000;
opacity: 0.8;
}
.loading-content{
position: relative;
z-index: 1;
display: flex;
flex-direction: column;
height: 100%;
width:100%;
justify-content: center;
align-items: center;
color:#fff;
}
.page{
margin: 1px auto -8px auto;
border: 9px solid transparent;
background-clip: content-box;
border-image: url(images/shadow.png) 9 9 repeat;
}
.page .loadingIcon{
width:100vw;
height: 100vh;
position: relative;
background-size: 100px 100px;
background-position: center center;
background-image: url(images/loading.gif);
background-repeat: no-repeat;
}
#viewerContainer {
position: absolute;
overflow: auto;
width: 100%;
top: 0;
bottom: 49px;
left: 0;
right: 0;
}

canvas {
margin: auto;
display: block;
}

/* 底部导航条 */
footer {

height: 49px;
position: absolute;
bottom: 0;
left: 0;
right: 0;
z-index: 1;

}
.footer-wrapper{
height: 100%;
width:100%;

background-image: url(images/toolbar_background.png);
box-shadow: 0 5px 8px rgba(50, 50, 50, 0.75);
display: flex;

}
.toolbarButton, 
.pageNumber{
flex:1;
border:none;
outline: none;
}
.pageNumber{
width:35%;
color:#fff;
display: flex;
justify-content: center;
align-items: center;
font-size:16px;
font-weight: 700;
background-color: transparent;
}
.toolbarButton.pageUp {
background-image: url(images/icon_previous_page.png);
background-repeat: no-repeat;
background-position: center center;
background-color: transparent;
}

.toolbarButton.pageDown {
background-image: url(images/icon_next_page.png);
background-repeat: no-repeat;
background-position: center center;
background-color: transparent;
}


.toolbarButton.zoomOut {
background-image: url(images/icon_zoom_out.png);
background-repeat: no-repeat;
background-position: center center;
background-color: transparent;
}

.toolbarButton.zoomIn {
background-image: url(images/icon_zoom_in.png);
background-repeat: no-repeat;
background-position: center center;
background-color: transparent;
}

.toolbarButton[disabled] {
opacity: 0.3;
}
/* 底部导航条 */



</style>

```



### 参考代码

```vue
import PDFJS from "pdfjs-dist";
import { TextLayerBuilder } from "pdfjs-dist/web/pdf_viewer";
import "pdfjs-dist/web/pdf_viewer.css";
PDFJS.GlobalWorkerOptions.workerSrc = "pdfjs-dist/build/pdf.worker.js";

var container;
export default {
name: "HelloWorld",
props: {
msg: String
},
mounted() {
this.$nextTick(() => {
let url =
"http://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf";
this.getPDF(url);
});
},
methods: {
async getPDF(url) {
let pdf = await PDFJS.getDocument(url)
container = container || document.querySelector('#container')
for(let i = 0; i < pdf.numPages; i++) {
try{
await this.rendPDF(pdf, i)
} catch(e) {
// console.error(e)
}
}
},
async renderPDF(pdf, num) {
let page = await pdf.getPage(num)
// 设置展示比例
let scale = 1.5;
let viewport = page.getViewport(scale);

let pageDiv = document.createElement('div');
pageDiv.setAttribute('id', 'page-' + (page.pageIndex + 1));
pageDiv.setAttribute('style', 'position: relative');
container.appendChild(pageDiv);

let canvas = document.createElement('canvas');
pageDiv.appendChild(canvas);
let context = canvas.getContext('2d');
canvas.height = viewport.height;
canvas.width = viewport.width;

let renderContext = {
canvasContext: context,
viewport: viewport
};

await page.render(renderContext);
let textContent = await page.getTextContent()
// 创建文本图层div
const textLayerDiv = document.createElement('div');
textLayerDiv.setAttribute('class', 'textLayer');
textLayerDiv.setAttribute('style', `width: ${viewport.width}px; margin: 0 auto;`)
// 将文本图层div添加至每页pdf的div中
pageDiv.appendChild(textLayerDiv);

// 创建新的TextLayerBuilder实例
var textLayer = new TextLayerBuilder({
textLayerDiv: textLayerDiv,
pageIndex: page.pageIndex,
viewport: viewport
});

textLayer.setTextContent(textContent);

textLayer.render();

}
}
};

```


