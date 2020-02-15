<template>
    <div id="app">
        <!-- Other properties are :title and :language -->
        <preview-box
                :min-screen-size="380"
                :snap-to-width="true"
                :show-guides="true"
                :iframe-src="'/card.html'">
            <template v-slot:setup>
                <div>Some setup text</div>
            </template>
            <template v-slot:copy-setup>
                &lt;!-- When on the setup screen, click the copy button and this data will be copied to the clipboard --&gt;
            </template>
            <template v-slot:code-snippet>
                <div v-pre>&lt;div&gt;This code is to be in a raw entity format for syntax highlighting&lt;/div&gt;</div>
            </template>
            <template v-slot:copy-code-snippet>
                <div v-pre>&lt;div&gt;When on the any other screen, click the copy button and this data will be copied to the clipboard &lt;/div&gt;</div>
            </template>
        </preview-box>
    </div>
</template>

<script>
    import PreviewBox from './components/PreviewBoxComponent.vue'

    export default {
        name: 'app',
        components: {
            'preview-box': PreviewBox
        }
    }
</script>
