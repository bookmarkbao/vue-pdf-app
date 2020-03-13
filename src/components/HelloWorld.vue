<template>
  <div class="hello viewerContainer" id="viewerContainer">
    <div class="headerFix">
      <div class="header-inner">
        <button @click="scalePlus">放大</button>
        <span>1/{{numPages}}</span>
        <button @click="scaleMinus">缩小</button>
      </div>
    </div>
    <div id="container" ref="container"></div>
  </div>
</template>

<script>
import PDFJS from "pdfjs-dist";
import { TextLayerBuilder } from "pdfjs-dist/web/pdf_viewer";
import "pdfjs-dist/web/pdf_viewer.css";
PDFJS.GlobalWorkerOptions.workerSrc = "pdfjs-dist/build/pdf.worker.js";
/* eslint-disable */ 
let canvasArr;
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data(){
    return {
      url: 'http://image.damoxueyuan.com/files/11.pdf',
      container:null,
      scaleObj:{},
      numPages:0,
      scale:1.5,
      canvasWidth: 750,
      pdf: null
    }
  },
  mounted() {
    this.$nextTick(() => {
        this.initPDF();
    });
  },
  methods: {
    async initPDF(){
      //  读取PDF文档
      this.pdf =       await this.getPDF(this.url);
      this.numPages =  this.pdf.numPages;
      this.container = this.container || document.querySelector('#container');

      this.handlePDFPage();
    },

    scaleMinus(){
        // this.handlePDFPage();
      this.canvasWidth = this.canvasWidth - 50;

      let canvasArr = canvasArr || this.$refs.container.querySelectorAll('canvas');
      for (let i = 0; i < canvasArr.length; i++) {
          canvasArr[i].style.width = this.canvasWidth+'px';        
      }
    },

    scalePlus(){
      this.canvasWidth = this.canvasWidth + 50;

      let canvasArr = canvasArr || this.$refs.container.querySelectorAll('canvas');
      for (let i = 0; i < canvasArr.length; i++) {
          canvasArr[i].style.width = this.canvasWidth+'px';        
      }
    },

    async handlePDFPage(){//开始处理PDF循环
        this.container.innerHTML = '';
        // debugger;
        for(let i = 0; i < this.numPages; i++) {
            try{
                await this.rendPDF(this.pdf, i)
            } catch(e) {
                // console.error(e)
            }
        }
    },

    async getPDF(url) {  // 读取pdf文档
        let pdf = await PDFJS.getDocument(url);
        return pdf;
    },

    async rendPDF(pdf, num) {
        let page = await pdf.getPage(num)
        // 设置展示比例
        let scale = 1.5;
        let viewport = page.getViewport(scale);

        let pageDiv = document.createElement('div');
        pageDiv.setAttribute('id', 'page-' + (page.pageIndex + 1));
        pageDiv.setAttribute('style', 'position: relative');
        this.container.appendChild(pageDiv);

        let canvas = document.createElement('canvas');
            // canvas.style.width=this.canvasWidth+'px';
        pageDiv.appendChild(canvas);
        let context = canvas.getContext('2d');
        canvas.height = viewport.height;
        canvas.width = viewport.width;
        
        let renderContext = {
            canvasContext: context,
            viewport: viewport
        };
        
        await page.render(renderContext);
        // debugger
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
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#viewerContainer {
    position: absolute;
    overflow: auto;
    width: 100%;
    top: 5rem;
    bottom: 4rem;
    left: 0;
    right: 0;
}
.headerFix{
  position: fixed;
  top:0;
  left:0;
  width:100%;
  z-index: 10000;
  height: 50px;
  line-height: 50px;
  background: #fff;

}
.header-inner{
  display: flex;
}
.header-inner span,
.header-inner button{
  flex:1;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
