<template>
    <div class="site-footer">

        <span xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/">
            <a property="dct:title" rel="cc:attributionURL" href="https://leobossmann.github.io">{{ title }}</a>
            by
            <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://github.com/leobossmann">Leo
                Bossmann</a>

            is licensed under

            <a href="http://creativecommons.org/licenses/by-nc-sa/4.0/?ref=chooser-v1" target="_blank"
                rel="license noopener noreferrer" style="display:inline-block;">CC BY-NC-SA 4.0
                <img src="/icons/cc.svg?ref=chooser-v1"><img src="/icons/by.svg?ref=chooser-v1"><img
                    src="/icons/nc.svg?ref=chooser-v1"><img src="/icons/sa.svg?ref=chooser-v1"></a>
        </span>
        <br>
        Powered by <a class="vitepress" target="_blank" href="//vitepress.vuejs.org/">VitePress</a>
        &mdash;
        Theme based on <a class="vitepress" target="_blank" href="//github.com/airene/vitepress-blog-pure">Airene's
            vitepress-blog-pure</a>
    </div>
</template>
<script lang="ts" setup>
import { useData } from 'vitepress'

const { site, theme } = useData()
const url = theme.value.website
const title = site.value.title

</script>

<style>
.site-footer {
    color: #888;
    text-align: center;
    font-size: 0.66rem;
    width: 100%;
    padding: 15px 0;
    overflow: auto;

    & a {
        color: var(--vp-c-text-1) !important;
        font-weight: 700;

        & img {
            display: initial;
            height:12px!important;
            margin-left:3px;
            vertical-align:text-bottom;
        }
    }
}
</style>
