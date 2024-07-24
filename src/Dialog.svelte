<svelte:options tag="fds-image-editor-compare-floating-toolbar"></svelte:options>
<script>

/**
 * the tool layer object
 * @type {object}
 */   
 export let tool_layer
export function refresh() {
    tool_layer=tool_layer
}
</script>
<!-- svelte-ignore a11y-click-events-have-key-events -->
<fds-image-editor-button
        class="btn"
        type="button"
        on:click={() => {
            tool_layer.component.sliderValue = 0
            tool_layer.component.updateSlider()
        }}
    ><div class="btn">0%</div>
    </fds-image-editor-button>    
<!-- svelte-ignore a11y-click-events-have-key-events -->
<fds-image-editor-button
        class="btn"
        type="button"
        on:click={() => {
            tool_layer.component.sliderValue = 50
            tool_layer.component.updateSlider()
        }}
    ><div class="btn">50%</div>
    </fds-image-editor-button>    
<!-- svelte-ignore a11y-click-events-have-key-events -->
<fds-image-editor-button
        class="btn"
        type="button"
        on:click={() => {
            tool_layer.component.sliderValue = 100
            tool_layer.component.updateSlider()
        }}
    ><div class="btn">100%</div>
    </fds-image-editor-button>            
<style>
    .btn {
        width: 40px;
        text-align: center;
    }
</style>