<template>
    <div id="app">
        <!-- Other properties are :title and :language -->
        <preview-box :min-screen-size="380" :iframe-src="'/card.html'">
            <template v-slot:setup>
                <!-- Some setup text -->
            </template>
            <template v-slot:copy-setup>
                <!-- When on the setup screen, click the copy button and this data will be copied to the clipboard -->
            </template>
            <template v-slot:code-snippet>
                <!-- This code is to be in a raw entity format for syntax highlighting -->
            </template>
            <template v-slot:copy-code-snippet>
                <!-- When on the any other screen, click the copy button and this data will be copied to the clipboard -->
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
