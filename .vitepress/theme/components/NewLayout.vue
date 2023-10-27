<template>
    <Layout/>
    <Copyright />
    <CookieConsent></CookieConsent>
</template>
<script setup>
import DefaultTheme from 'vitepress/theme'
import Copyright from './Copyright.vue'
import CookieConsent from './CookieConsent.vue'
const { Layout } = DefaultTheme

</script>
