<template>
    <div id="app">

        <h1 class="mb-8 text-2xl text-gray-700">Snap guides with visible markers</h1>

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

        <h1 class="mb-8 text-2xl text-gray-700">Visible markers without snap guides</h1>

        <preview-box
            :min-screen-size="380"
            :snap-to-width="false"
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

        <h1 class="mb-8 text-2xl text-gray-700">Basic</h1>

        <preview-box
            :min-screen-size="380"
            :snap-to-width="false"
            :show-guides="false"
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
