<script lang="ts" setup>
import { onMounted, ref, watch, nextTick } from 'vue'
import { useData } from 'vitepress'
const utterancesRef = ref()
const { theme, isDark } = useData()

const toggleUi = ref()

const loadUtterances = () => {
    toggleUi.value.remove();
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
    <div class="utteranceTeoggle" ref="utterancesRef">
        <div ref="toggleUi">
            <button @click="loadUtterances">load utterances comments</button>
            <br>
            <small>Clicking this button loads third-party content from utteranc.es and github.com</small>
        </div>
    </div>
</template>

<style>
/*global  style*/
.utterances {
    max-width: inherit !important;
}

.utteranceTeoggle {
    text-align: center;
    margin: 4rem 0 0 0;

    & button {
        padding: 10px 20px;
        border-radius: 5px;
        border: 1px solid var(--vp-c-brand);
        cursor: pointer;
    }

    & small {
        font-size: 65%;
    }
}
</style>
