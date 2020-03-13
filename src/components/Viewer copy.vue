<template>
  <div>
    <div id="viewerContainer">
      <div id="viewer" class="pdfViewer">
        <div
          class="page"
          data-page-number="1"
          data-loaded="true"
          style="width: 695px; height: 774px;"
        >
          <div class="canvasWrapper" style="width: 695px; height: 774px;">
            <canvas
              width="1992"
              height="2214"
              aria-label="Page 1"
              style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);"
            ></canvas>
          </div>
        </div>
        <div
          class="page"
          data-page-number="2"
          data-loaded="true"
          style="width: 695px; height: 774px;"
        >
          <div class="canvasWrapper" style="width: 695px; height: 774px;">
            <canvas
              width="1992"
              height="2214"
              aria-label="Page 2"
              style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);"
            ></canvas>
          </div>
        </div>
        <div
          class="page"
          data-page-number="3"
          data-loaded="true"
          style="width: 695px; height: 774px;"
        >
          <div class="canvasWrapper" style="width: 695px; height: 774px;">
            <canvas
              width="1992"
              height="2214"
              aria-label="Page 3"
              style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);"
            ></canvas>
          </div>
        </div>
        <div
          class="page"
          data-page-number="4"
          data-loaded="true"
          style="width: 695px; height: 774px;"
        >
          <div class="canvasWrapper" style="width: 695px; height: 774px;">
            <canvas
              width="1992"
              height="2214"
              aria-label="Page 4"
              style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);"
            ></canvas>
          </div>
        </div>
        <div
          class="page"
          data-page-number="5"
          data-loaded="true"
          style="width: 695px; height: 774px;"
        >
          <div class="canvasWrapper" style="width: 695px; height: 774px;">
            <canvas
              width="1992"
              height="2214"
              aria-label="Page 5"
              style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);"
            ></canvas>
          </div>
        </div>
        <div
          class="page"
          data-page-number="6"
          data-loaded="true"
          style="width: 695px; height: 774px;"
        >
          <div class="canvasWrapper" style="width: 695px; height: 774px;">
            <canvas
              width="1992"
              height="2214"
              aria-label="Page 6"
              style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);"
            ></canvas>
          </div>
        </div>
        <div
          class="page"
          data-page-number="7"
          data-loaded="true"
          style="width: 695px; height: 774px;"
        >
          <div class="canvasWrapper" style="width: 695px; height: 774px;">
            <canvas
              width="1992"
              height="2214"
              aria-label="Page 7"
              style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);"
            ></canvas>
          </div>
        </div>
        <div
          class="page"
          data-page-number="8"
          data-loaded="true"
          style="width: 695px; height: 774px;"
        >
          <div class="canvasWrapper" style="width: 695px; height: 774px;">
            <canvas
              width="1992"
              height="2214"
              aria-label="Page 8"
              style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);"
            ></canvas>
          </div>
        </div>
        <div
          class="page"
          data-page-number="9"
          data-loaded="true"
          style="width: 695px; height: 774px;"
        >
          <div class="loadingIcon"></div>
          <div class="canvasWrapper" style="width: 695px; height: 774px;">
            <canvas
              width="1992"
              height="2214"
              aria-label="Page 9"
              style="width: 695px; height: 774px; transform: rotate(0deg) scale(1, 1);"
            ></canvas>
          </div>
        </div>
        <div class="page" data-page-number="10" style="width: 695px; height: 774px;">
          <div class="loadingIcon"></div>
        </div>
      </div>
    </div>
    <footer>
      <button class="toolbarButton pageUp" title="Previous Page" id="previous"></button>
      <button class="toolbarButton pageDown" title="Next Page" id="next"></button>

      <input
        type="number"
        id="pageNumber"
        class="toolbarField pageNumber"
        value="1"
        size="4"
        min="1"
      />

      <button class="toolbarButton zoomOut" title="Zoom Out" id="zoomOut"></button>
      <button class="toolbarButton zoomIn" title="Zoom In" id="zoomIn"></button>
    </footer>
  </div>
</template>

<script>
export default {};
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
  padding-bottom: 5rem;
}
header {
  background-color: rgba(244, 244, 244, 1);
}

header h1 {
  border-bottom: 1px solid rgba(216, 216, 216, 1);
  color: rgba(133, 133, 133, 1);
  font-size: 23px;
  font-style: italic;
  font-weight: normal;
  overflow: hidden;
  padding: 10px;
  text-align: center;
  text-overflow: ellipsis;
  white-space: nowrap;
}

body {
  background: url(images/document_bg.png);
  color: rgba(255, 255, 255, 1);
  font-family: sans-serif;
  font-size: 10px;
  height: 100%;
  width: 100%;
  /* overflow: hidden; */
  padding-bottom: 5rem;
}

section {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  font-size: 2rem;
}

footer {
  background-image: url(images/toolbar_background.png);
  height: 4rem;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
  box-shadow: 0 -0.2rem 0.5rem rgba(50, 50, 50, 0.75);
}

.toolbarButton {
  display: block;
  padding: 0;
  margin: 0;
  border-width: 0;
  background-position: center center;
  background-repeat: no-repeat;
  background-color: rgba(0, 0, 0, 0);
}

.toolbarButton.pageUp {
  position: absolute;
  width: 18%;
  height: 100%;
  left: 0;
  background-image: url(images/icon_previous_page.png);
  background-size: 2rem;
}

.toolbarButton.pageDown {
  position: absolute;
  width: 18%;
  height: 100%;
  left: 18%;
  background-image: url(images/icon_next_page.png);
  background-size: 2rem;
}

#pageNumber {
  -moz-appearance: textfield; /* hides the spinner in moz */
  position: absolute;
  width: 28%;
  height: 100%;
  left: 36%;
  text-align: center;
  border: 0;
  background-color: rgba(0, 0, 0, 0);
  font-size: 1.2rem;
  color: rgba(255, 255, 255, 1);
  background-image: url(images/div_line_left.png),
    url(images/div_line_right.png);
  background-repeat: no-repeat;
  background-position: left, right;
  background-size: 0.2rem, 0.2rem;
}

.toolbarButton.zoomOut {
  position: absolute;
  width: 18%;
  height: 100%;
  left: 64%;
  background-image: url(images/icon_zoom_out.png);
  background-size: 2.4rem;
}

.toolbarButton.zoomIn {
  position: absolute;
  width: 18%;
  height: 100%;
  left: 82%;
  background-image: url(images/icon_zoom_in.png);
  background-size: 2.4rem;
}

.toolbarButton[disabled] {
  opacity: 0.3;
}

.hidden {
  display: none;
}
[hidden] {
  display: none !important;
}

#viewerContainer {
  position: absolute;
  overflow: auto;
  width: 100%;
  top: 0;
  bottom: 4rem;
  left: 0;
  right: 0;
}

canvas {
  margin: auto;
  display: block;
}

.pdfViewer .page .loadingIcon {
  width: 2.9rem;
  height: 2.9rem;
  background: url("images/spinner.png") no-repeat left top / 38rem;
  border: medium none;
  animation: 1s steps(10, end) 0s normal none infinite moveDefault;
  display: block;
  position: absolute;
  top: calc((100% - 2.9rem) / 2);
  left: calc((100% - 2.9rem) / 2);
}

@keyframes moveDefault {
  from {
    background-position: 0 top;
  }

  to {
    background-position: -39rem top;
  }
}

#loadingBar {
  position: relative;
  height: 0.6rem;
  background-color: rgba(51, 51, 51, 1);
  border-bottom: 1px solid rgba(51, 51, 51, 1);
  margin-top: 5rem;
}

#loadingBar .progress {
  position: absolute;
  left: 0;
  width: 0;
  height: 100%;
  background-color: rgba(221, 221, 221, 1);
  overflow: hidden;
  transition: width 200ms;
}

@keyframes progressIndeterminate {
  0% {
    left: 0;
  }
  50% {
    left: 100%;
  }
  100% {
    left: 100%;
  }
}

#loadingBar .progress.indeterminate {
  background-color: rgba(153, 153, 153, 1);
  transition: none;
}

#loadingBar .indeterminate .glimmer {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 5rem;
  background-image: linear-gradient(
    to right,
    rgba(153, 153, 153, 1) 0%,
    rgba(255, 255, 255, 1) 50%,
    rgba(153, 153, 153, 1) 100%
  );
  background-size: 100% 100%;
  background-repeat: no-repeat;
  animation: progressIndeterminate 2s linear infinite;
}

#errorWrapper {
  background: none repeat scroll 0 0 rgba(255, 85, 85, 1);
  color: rgba(255, 255, 255, 1);
  left: 0;
  position: absolute;
  right: 0;
  top: 3.2rem;
  z-index: 1000;
  padding: 0.3rem;
  font-size: 0.8em;
}

#errorMessageLeft {
  float: left;
}

#errorMessageRight {
  float: right;
}

#errorMoreInfo {
  background-color: rgba(255, 255, 255, 1);
  color: rgba(0, 0, 0, 1);
  padding: 0.3rem;
  margin: 0.3rem;
  width: 98%;
}

/* Copyright 2014 Mozilla Foundation
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

.textLayer {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  overflow: hidden;
  opacity: 0.2;
  line-height: 1;
}

.textLayer > span {
  color: transparent;
  position: absolute;
  white-space: pre;
  cursor: text;
  transform-origin: 0% 0%;
}

.textLayer .highlight {
  margin: -1px;
  padding: 1px;
  background-color: rgba(180, 0, 170, 1);
  border-radius: 4px;
}

.textLayer .highlight.begin {
  border-radius: 4px 0px 0px 4px;
}

.textLayer .highlight.end {
  border-radius: 0px 4px 4px 0px;
}

.textLayer .highlight.middle {
  border-radius: 0px;
}

.textLayer .highlight.selected {
  background-color: rgba(0, 100, 0, 1);
}

.textLayer ::-moz-selection {
  background: rgba(0, 0, 255, 1);
}

.textLayer ::selection {
  background: rgba(0, 0, 255, 1);
}

.textLayer .endOfContent {
  display: block;
  position: absolute;
  left: 0px;
  top: 100%;
  right: 0px;
  bottom: 0px;
  z-index: -1;
  cursor: default;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.textLayer .endOfContent.active {
  top: 0px;
}

.annotationLayer section {
  position: absolute;
}

.annotationLayer .linkAnnotation > a,
.annotationLayer .buttonWidgetAnnotation.pushButton > a {
  position: absolute;
  font-size: 1em;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.annotationLayer .linkAnnotation > a:hover,
.annotationLayer .buttonWidgetAnnotation.pushButton > a:hover {
  opacity: 0.2;
  background: rgba(255, 255, 0, 1);
  box-shadow: 0px 2px 10px rgba(255, 255, 0, 1);
}

.annotationLayer .textAnnotation img {
  position: absolute;
  cursor: pointer;
}

.annotationLayer .textWidgetAnnotation input,
.annotationLayer .textWidgetAnnotation textarea,
.annotationLayer .choiceWidgetAnnotation select,
.annotationLayer .buttonWidgetAnnotation.checkBox input,
.annotationLayer .buttonWidgetAnnotation.radioButton input {
  background-color: rgba(0, 54, 255, 0.13);
  border: 1px solid transparent;
  box-sizing: border-box;
  font-size: 9px;
  height: 100%;
  margin: 0;
  padding: 0 3px;
  vertical-align: top;
  width: 100%;
}

.annotationLayer .choiceWidgetAnnotation select option {
  padding: 0;
}

.annotationLayer .buttonWidgetAnnotation.radioButton input {
  border-radius: 50%;
}

.annotationLayer .textWidgetAnnotation textarea {
  font: message-box;
  font-size: 9px;
  resize: none;
}

.annotationLayer .textWidgetAnnotation input[disabled],
.annotationLayer .textWidgetAnnotation textarea[disabled],
.annotationLayer .choiceWidgetAnnotation select[disabled],
.annotationLayer .buttonWidgetAnnotation.checkBox input[disabled],
.annotationLayer .buttonWidgetAnnotation.radioButton input[disabled] {
  background: none;
  border: 1px solid transparent;
  cursor: not-allowed;
}

.annotationLayer .textWidgetAnnotation input:hover,
.annotationLayer .textWidgetAnnotation textarea:hover,
.annotationLayer .choiceWidgetAnnotation select:hover,
.annotationLayer .buttonWidgetAnnotation.checkBox input:hover,
.annotationLayer .buttonWidgetAnnotation.radioButton input:hover {
  border: 1px solid rgba(0, 0, 0, 1);
}

.annotationLayer .textWidgetAnnotation input:focus,
.annotationLayer .textWidgetAnnotation textarea:focus,
.annotationLayer .choiceWidgetAnnotation select:focus {
  background: none;
  border: 1px solid transparent;
}

.annotationLayer .buttonWidgetAnnotation.checkBox input:checked:before,
.annotationLayer .buttonWidgetAnnotation.checkBox input:checked:after,
.annotationLayer .buttonWidgetAnnotation.radioButton input:checked:before {
  background-color: rgba(0, 0, 0, 1);
  content: "";
  display: block;
  position: absolute;
}

.annotationLayer .buttonWidgetAnnotation.checkBox input:checked:before,
.annotationLayer .buttonWidgetAnnotation.checkBox input:checked:after {
  height: 80%;
  left: 45%;
  width: 1px;
}

.annotationLayer .buttonWidgetAnnotation.checkBox input:checked:before {
  transform: rotate(45deg);
}

.annotationLayer .buttonWidgetAnnotation.checkBox input:checked:after {
  transform: rotate(-45deg);
}

.annotationLayer .buttonWidgetAnnotation.radioButton input:checked:before {
  border-radius: 50%;
  height: 50%;
  left: 30%;
  top: 20%;
  width: 50%;
}

.annotationLayer .textWidgetAnnotation input.comb {
  font-family: monospace;
  padding-left: 2px;
  padding-right: 0;
}

.annotationLayer .textWidgetAnnotation input.comb:focus {
  /*
   * Letter spacing is placed on the right side of each character. Hence, the
   * letter spacing of the last character may be placed outside the visible
   * area, causing horizontal scrolling. We avoid this by extending the width
   * when the element has focus and revert this when it loses focus.
   */
  width: 115%;
}

.annotationLayer .buttonWidgetAnnotation.checkBox input,
.annotationLayer .buttonWidgetAnnotation.radioButton input {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  padding: 0;
}

.annotationLayer .popupWrapper {
  position: absolute;
  width: 20em;
}

.annotationLayer .popup {
  position: absolute;
  z-index: 200;
  max-width: 20em;
  background-color: rgba(255, 255, 153, 1);
  box-shadow: 0px 2px 5px rgba(136, 136, 136, 1);
  border-radius: 2px;
  padding: 6px;
  margin-left: 5px;
  cursor: pointer;
  font: message-box;
  font-size: 9px;
  word-wrap: break-word;
}

.annotationLayer .popup > * {
  font-size: 9px;
}

.annotationLayer .popup h1 {
  display: inline-block;
}

.annotationLayer .popup span {
  display: inline-block;
  margin-left: 5px;
}

.annotationLayer .popup p {
  border-top: 1px solid rgba(51, 51, 51, 1);
  margin-top: 2px;
  padding-top: 2px;
}

.annotationLayer .highlightAnnotation,
.annotationLayer .underlineAnnotation,
.annotationLayer .squigglyAnnotation,
.annotationLayer .strikeoutAnnotation,
.annotationLayer .freeTextAnnotation,
.annotationLayer .lineAnnotation svg line,
.annotationLayer .squareAnnotation svg rect,
.annotationLayer .circleAnnotation svg ellipse,
.annotationLayer .polylineAnnotation svg polyline,
.annotationLayer .polygonAnnotation svg polygon,
.annotationLayer .caretAnnotation,
.annotationLayer .inkAnnotation svg polyline,
.annotationLayer .stampAnnotation,
.annotationLayer .fileAttachmentAnnotation {
  cursor: pointer;
}

.pdfViewer .canvasWrapper {
  overflow: hidden;
}

.pdfViewer .page {
  direction: ltr;
  width: 816px;
  height: 1056px;
  margin: 1px auto -8px auto;
  position: relative;
  overflow: visible;
  border: 9px solid transparent;
  background-clip: content-box;
  -o-border-image: url(images/shadow.png) 9 9 repeat;
  border-image: url(images/shadow.png) 9 9 repeat;
  background-color: rgba(255, 255, 255, 1);
}

.pdfViewer.removePageBorders .page {
  margin: 0px auto 10px auto;
  border: none;
}

.pdfViewer.singlePageView {
  display: inline-block;
}

.pdfViewer.singlePageView .page {
  margin: 0;
  border: none;
}

.pdfViewer.scrollHorizontal,
.pdfViewer.scrollWrapped,
.spread {
  margin-left: 3.5px;
  margin-right: 3.5px;
  text-align: center;
}

.pdfViewer.scrollHorizontal,
.spread {
  white-space: nowrap;
}

.pdfViewer.removePageBorders,
.pdfViewer.scrollHorizontal .spread,
.pdfViewer.scrollWrapped .spread {
  margin-left: 0;
  margin-right: 0;
}

.spread .page,
.pdfViewer.scrollHorizontal .page,
.pdfViewer.scrollWrapped .page,
.pdfViewer.scrollHorizontal .spread,
.pdfViewer.scrollWrapped .spread {
  display: inline-block;
  vertical-align: middle;
}

.spread .page,
.pdfViewer.scrollHorizontal .page,
.pdfViewer.scrollWrapped .page {
  margin-left: -3.5px;
  margin-right: -3.5px;
}

.pdfViewer.removePageBorders .spread .page,
.pdfViewer.removePageBorders.scrollHorizontal .page,
.pdfViewer.removePageBorders.scrollWrapped .page {
  margin-left: 5px;
  margin-right: 5px;
}

.pdfViewer .page canvas {
  margin: 0;
  display: block;
}

.pdfViewer .page canvas[hidden] {
  display: none;
}

.pdfViewer .page .loadingIcon {
  position: absolute;
  display: block;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  /* background: url('images/loading-icon.gif') center no-repeat; */
}

.pdfPresentationMode .pdfViewer {
  margin-left: 0;
  margin-right: 0;
}

.pdfPresentationMode .pdfViewer .page,
.pdfPresentationMode .pdfViewer .spread {
  display: block;
}

.pdfPresentationMode .pdfViewer .page,
.pdfPresentationMode .pdfViewer.removePageBorders .page {
  margin-left: auto;
  margin-right: auto;
}

.pdfPresentationMode:-ms-fullscreen .pdfViewer .page {
  margin-bottom: 100% !important;
}

.pdfPresentationMode:-webkit-full-screen .pdfViewer .page {
  margin-bottom: 100%;
  border: 0;
}

.pdfPresentationMode:-moz-full-screen .pdfViewer .page {
  margin-bottom: 100%;
  border: 0;
}

.pdfPresentationMode:fullscreen .pdfViewer .page {
  margin-bottom: 100%;
  border: 0;
}
</style>
