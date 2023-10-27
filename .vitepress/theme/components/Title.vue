<template>
    <p style="margin-bottom: 0;">
        <small>
            <span v-if="page.frontmatter.date">Posted on
                <span>{{ new Date(page.frontmatter.date).toLocaleDateString() }}</span>
            </span>
            <span v-if="page.frontmatter.lastUpdated"> (updated on
                <span>{{ new Date(page.frontmatter.lastUpdated).toLocaleDateString() }}</span>)
            </span>

            <span v-if="page.frontmatter.tags?.length > 0">

                in
                <span v-for="(item, index) in page.frontmatter.tags">
                    <a :href="withBase(`/pages/tags.html?tag=${item}`)">{{ item }}</a>
                    <template v-if="index < page.frontmatter.tags.length - 1">, </template> 
                </span> 
            </span>
        </small>
    </p>
    <h1 class="title" style="padding-top:0;">
        <template v-if="!!slots.default">
            <slot />
        </template>
        <template v-else>
            {{ page.title }}
        </template>
    </h1>
</template>
<script setup>
import { useSlots } from 'vue'
import { useData, withBase } from 'vitepress'
const slots = useSlots();
const { page } = useData();
</script>