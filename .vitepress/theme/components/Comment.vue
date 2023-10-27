<script lang="ts" setup>
import { onMounted, ref, watch, nextTick } from 'vue'
import { useData } from 'vitepress'
const utterancesRef = ref()
const { theme, isDark } = useData()

const loadButton = ref()

const loadUtterances = () => {
    loadButton.value.remove();
    let { repo, issueTerm = 'pathname' } = theme.value.comment;
    if (repo) {
        let utterances = document.createElement('script')
        utterances.async = true
        utterances.setAttribute('src', 'https://utteranc.es/client.js')
        utterances.setAttribute('repo', repo)
        utterances.setAttribute('issue-term', issueTerm)
        utterances.setAttribute('theme', isDark.value ? 'github-dark' : 'github-light')
        utterances.setAttribute('crossorigin', 'anonymous')
        utterancesRef.value.appendChild(utterances)
    }
}

onMounted(() => {
    nextTick(() => {

        //hack method to change utterances theme when change site theme
        watch(isDark, (newVal, oldVal) => {
            if (newVal !== oldVal) location.replace(location.href)
        })
    })
})
</script>

<template>
    <div ref="utterancesRef">
        <button @click="loadUtterances" ref="loadButton">load utterances comments</button>
    </div>
</template>

<style>
/*global  style*/
.utterances {
    max-width: inherit !important;
}
</style>
