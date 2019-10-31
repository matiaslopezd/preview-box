<template>
    <div class="pb" :class="{ 'pb--full-screen': fullScreen }">
        <div class="pb__hidden-copiers">
            <textarea ref="copyCodeHTML" class="outline-none" :id="referenceId"></textarea>
            <textarea ref="copySetup" class="outline-none" :id="copySetupId"></textarea>
        </div>
        <div class="pb__container">
            <div class="pb__header">
                <h3 class="pb__header__heading">{{ title }}</h3>
                <div class="pb__header__inner">
                    <div class="pb__header__tabs">
                        <button :class="{ 'pb__header__tab--active': activeTabIndex === 0 }" @click="showTab(0)" type="button" class="pb__header__tab">Setup</button>
                        <button :class="{ 'pb__header__tab--active': activeTabIndex === 1 }" @click="showTab(1)" type="button" class="pb__header__tab">Preview</button>
                        <button :class="{ 'pb__header__tab--active': activeTabIndex === 2 }" @click="showTab(2)" type="button" class="pb__header__tab">Code</button>
                    </div>
                    <button v-if="activeTabIndex !== 0" aria-label="Copy HTML" data-balloon-pos="left" type="button" :data-clipboard-target="'#' + referenceId" class="copy pb__header__button">
                        <svg class="h-6 w-6 fill-current" viewBox="0 0 24 24">
                            <path fill-rule="evenodd" clip-rule="evenodd" d="M13 2a1 1 0 0 1 1 1 1 1 0 0 0 1 1 1 1 0 1 1 0 2h-5a1 1 0 0 1 0-2 1 1 0 0 0 1-1 1 1 0 0 1 1-1h1zM8 5H7a1 1 0 0 0-1 1v13a1 1 0 0 0 1 1h11a1 1 0 0 0 1-1V6a1 1 0 0 0-1-1h-1a2 2 0 0 1-2 2h-5a2 2 0 0 1-2-2zm1 5h7v1H9v-1zm7 2H9v1h7v-1zm-7 3v-1h4v1H9z"></path>
                        </svg>
                    </button>
                    <button v-if="activeTabIndex === 0" aria-label="Copy CDN" data-balloon-pos="left" type="button" :data-clipboard-target="'#' + copySetupId" class="copy pb__header__button">
                        <svg class="h-6 w-6 fill-current" viewBox="0 0 24 24">
                            <path fill-rule="evenodd" clip-rule="evenodd" d="M13 2a1 1 0 0 1 1 1 1 1 0 0 0 1 1 1 1 0 1 1 0 2h-5a1 1 0 0 1 0-2 1 1 0 0 0 1-1 1 1 0 0 1 1-1h1zM8 5H7a1 1 0 0 0-1 1v13a1 1 0 0 0 1 1h11a1 1 0 0 0 1-1V6a1 1 0 0 0-1-1h-1a2 2 0 0 1-2 2h-5a2 2 0 0 1-2-2zm1 5h7v1H9v-1zm7 2H9v1h7v-1zm-7 3v-1h4v1H9z"></path>
                        </svg>
                    </button>
                    <a aria-label="Open in new tab" data-balloon-pos="left" :href="iframeSrc" target="_blank" class="pb__header__button">
                        <svg class="pb__header__svg" fill="currentColor" viewBox="0 0 24 24"><path d="M19 6.41L8.7 16.71a1 1 0 1 1-1.4-1.42L17.58 5H14a1 1 0 0 1 0-2h6a1 1 0 0 1 1 1v6a1 1 0 0 1-2 0V6.41zM17 14a1 1 0 0 1 2 0v5a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V7c0-1.1.9-2 2-2h5a1 1 0 0 1 0 2H5v12h12v-5z"></path></svg>
                    </a>
                    <a @click="toggleFullscreen" aria-label="Toggle fullscreen" data-balloon-pos="left" href="#" class="pb__header__button">
                        <svg class="pb__header__svg" fill="currentColor" viewBox="0 0 24 24"><path d="M4.207 17.207L7 20H0v-7l2.793 2.793 4.328-4.329 1.415 1.415-4.329 4.328zM15.793 2.793L13 0h7v7l-2.793-2.793-4.328 4.329-1.415-1.415 4.329-4.328zm0 14.414L13 20h7v-7l-2.793 2.793-4.328-4.329-1.415 1.415 4.329 4.328zM4.207 2.793L7 0H0v7l2.793-2.793L7.12 8.536 8.536 7.12 4.207 2.793z"/></svg>
                    </a>
                </div>
            </div>
            <div ref="container" class="pb__container__tabs">
                <div class="pb__container__tab pb__container__tab--setup" v-show="activeTabIndex === 0">
                    <slot name="setup"></slot>
                </div>
                <div v-show="activeTabIndex === 1" class="pb__container__tab pb__container__tab--iframe">
                    <iframe ref="iframe" height="600" :src="iframeSrc" class="pb__container__iframe"></iframe>
                    <div v-if="moving" class="pb__container__iframe-shield"></div>
                    <button ref="handle" v-on:mousedown="onMouseDown" class="pb__container__iframe-handle grabbable">
                        <svg class="pb__container__handle-svg" fill="currentColor" viewBox="0 0 24 24"><path d="M8 5h2v14H8zM14 5h2v14h-2z"></path></svg>
                    </button>
                </div>
                <div class="pb__container__tab pb__container__tab--code" v-show="activeTabIndex === 2">
                    <div class="relative">
                        <pre class="pb__container__syntax-highlighter-outer" :class="'language-' + language">
                            <code ref="codeSnippet" class="pb__container__syntax-highlighter-inner"><slot name="code-snippet"></slot></code>
                        </pre>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue';
    import ClipboardJS from 'clipboard';
    import Prism from 'prismjs';
    import 'balloon-css';
    const d = window.document;

    export default {
        name: 'preview-box',
        props: {
            language: {
                required: false,
                type: String,
                default: 'html'
            },
            iframeSrc: {
                required: true,
                type: String
            },
            minScreenSize: {
                required: true,
                type: Number
            },
            title: {
                required: false,
                type: String,
                default: ''
            }
        },
        data () {
            return {
                referenceId: null,
                copySetupId: null,
                position1: 0,
                position2: 0,
                moving: false,
                maxWidth: 0,
                activeTabIndex: 1,
                fullScreen: false
            }
        },
        mounted() {
            document.addEventListener('mousemove', this.onMouseMove);
            document.addEventListener('mouseup', this.onMouseUp);
            this.maxWidth = this.$refs.iframe.getBoundingClientRect().width;
            this.$refs.copyCodeHTML.innerHTML = this.$slots['copy-code-snippet'][0].text;
            this.$refs.copySetup.innerHTML = this.$slots['copy-setup'][0].text;
            this.referenceId = Math.random().toString(36).replace(/[^a-z]+/g, '').substr(2, 10);
            this.copySetupId = Math.random().toString(36).replace(/[^a-z]+/g, '').substr(2, 10);
            Prism.highlightAll();

            new ClipboardJS('.copy');
            d.querySelectorAll('.copy').forEach(function(btn){
                btn.addEventListener('click', function(){
                    this.setAttribute('aria-label', 'COPIED!');
                    this.classList.add('bounceIn');
                    setTimeout(function(){
                        this.setAttribute('aria-label', 'Copy HTML');
                        this.classList.remove('bounceIn');
                    }.bind(this), 1000);
                });
            });
        },
        methods: {
            onMouseDown(e) {
                e.preventDefault();
                this.position2 = e.clientX;
                this.moving = true;
            },
            onMouseMove(e) {
                e.preventDefault();
                this.position1 = this.position2 - e.clientX;
                this.position2 = e.clientX;
                if(this.moving) {
                    this.updatePosition();
                }
            },
            onMouseUp() {
                this.moving = false;
            },
            updatePosition() {
                let width = this.$refs.iframe.getBoundingClientRect().width;
                let calcWidth = width - this.position1;
                calcWidth = calcWidth < this.minScreenSize ? this.minScreenSize : (calcWidth > this.maxWidth ? this.maxWidth : calcWidth);

                this.$refs.iframe.style.width = calcWidth + 'px';
                this.$refs.handle.style.left = calcWidth + 'px';
                this.$refs.handle.style.right = 0;
            },
            showTab(index) {
                this.activeTabIndex = parseInt(index);
                this.$emit('openedTab', this.activeTabIndex);
            },
            toggleFullscreen(e) {
                e.preventDefault();
                this.$refs.iframe.focus();
                this.fullScreen = ! this.fullScreen;
                Vue.nextTick(() => {
                    this.maxWidth = this.$refs.container.getBoundingClientRect().width - 16;
                    this.updatePosition();
                });
            }
        }
    }
</script>

<style>
@import '~prismjs/themes/prism-tomorrow.css';

.grabbable {
    cursor: move;
    cursor: grab;
    cursor: -moz-grab;
    cursor: -webkit-grab;
}

.grabbable:active {
    cursor: grabbing;
    cursor: -moz-grabbing;
    cursor: -webkit-grabbing;
}

.pb--full-screen .pb__container__tabs {
    height: calc(100vh - 84px);
}

.pb--full-screen .pb__container__tab,
.pb--full-screen .pb__container__iframe,
.pb--full-screen .pb__container__syntax-highlighter-outer,
.pb--full-screen .pb__container__tab--code > div {
    height:100%;
}
</style>

