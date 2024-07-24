<svelte:options tag="fds-image-editor-compare"></svelte:options>
<script>
import { onMount } from 'svelte';

export let sliderValue = 50
let canvas
let topImage
let isDragging = false
export let tool_layer
// needed otherwise it will be not availabe in bundle
// eslint-disable-next-line no-unused-vars
import Dialog from './Dialog.svelte'
$: if (topImage) {
  updateSlider()
}

function handleMouseDown() {
  isDragging = true;
}

function handleMouseUp() {
  isDragging = false;
}
export function updateSlider() {
  topImage.style.clipPath = `inset(0 ${100 - sliderValue}% 0 0)`;

}

function handleMouseMove(event) {
  if (isDragging && canvas) {
    let coords=canvas.mouseCoords(event)
    const x = parseInt(coords.x)
    sliderValue = (x / canvas.width) * 100
    sliderValue = Math.max(0, Math.min(100, sliderValue))
    updateSlider()
  }
}
let canvasWidthZoomed,canvasHeightZoomed
export function refresh() {
  if (!globalThis.gyre || !globalThis.gyre.paletteValues || !globalThis.gyre.paletteValues.selectedLayer) return
  topImage=globalThis.gyre.paletteValues.selectedLayer.element.getElementsByTagName("img")[0]
  canvas=globalThis.gyre.canvas             // canvas API
  canvasWidthZoomed=canvas.calcPosition(canvas.width)        
  canvasHeightZoomed=canvas.calcPosition(canvas.height)  
}
onMount(() => {

  return () => {  // removing tag from DOM tree and it will be called by switching the tool so restore style here
    topImage.style.removeProperty('clip-path') 
  }
})
</script>

<div class="slider-container"  style="width:{canvasWidthZoomed}px;height:{canvasHeightZoomed}px;">
  <div 
    class="canvas" 
    bind:this={canvas}
    on:mousedown={handleMouseDown}
    on:mouseup={handleMouseUp}
    on:mouseleave={handleMouseUp}
    on:mousemove={handleMouseMove}
    style="width:{canvasWidthZoomed}px;height:{canvasHeightZoomed}px;"
  >
    <div class="slider-handle" style="left: {sliderValue}%;height:{canvasHeightZoomed}px">
      <div class="slider-button"></div>
    </div>
  </div>
</div>

<style>
.slider-container {
  position: absolute;
  margin: 0 auto;
  left:0;
  top:0;
}

.canvas {
  position: relative;
  overflow: hidden;
  cursor: col-resize;
  user-select: none;
}

.slider-handle {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 3px;
  background: white;
  cursor: col-resize;
}

.slider-button {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 20px;
  height: 20px;
  background: white;
  border: 2px solid #000;
  border-radius: 50%;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
}


.slider-button::before {
  left: 25%;
  transform: translateY(-50%) rotate(-45deg);
}

.slider-button::after {
  right: 25%;
  transform: translateY(-50%) rotate(45deg);
}
</style>