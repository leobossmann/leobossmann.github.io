<template>
    <div id="cookieConsent" ref="div" :style="`display: ${isOpen ? 'block' : 'none'};`">
        <div class="backdrop"></div>
        <div class="foreground">
            <a href="#" @click.stop.prevent="isOpen = false" class="close-button">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-x-lg"
                    viewBox="0 0 16 16">
                    <path
                        d="M2.146 2.854a.5.5 0 1 1 .708-.708L8 7.293l5.146-5.147a.5.5 0 0 1 .708.708L8.707 8l5.147 5.146a.5.5 0 0 1-.708.708L8 8.707l-5.146 5.147a.5.5 0 0 1-.708-.708L7.293 8 2.146 2.854Z" />
                </svg>
            </a>
            <div>
                <div>
                    This website uses Google Fonts
                    (<code>{{ state.fonts ? 'accepted' : 'not accepted' }}</code>)
                    and
                    Google Analytics
                    (<code>{{ state.analytics ? 'accepted' : 'not accepted' }}</code>)
                    . <a :href="withBase('/pages/privacy-policy.html')" @click="isOpen = false">Privacy Policy.</a>
                </div>
            </div>
            <div>



                <button @click="accept([])" class="accept-none">Accept None</button>
                <button @click="accept(['fonts'])" class="accept-fonts">Accept Fonts</button>
                <button @click="accept(['fonts', 'analytics'])" class="accept-all">Accept All</button>
            </div>
            <div>
                You can change your settings later by clicking the shield icon in the navigation.
            </div>
        </div>
    </div>
</template>

<script setup>

import { withBase, useData } from 'vitepress';
import { ref, onMounted } from 'vue';


const div = ref(null);
const isOpen = ref(false);

const fontCSS = `
<style>
  @import url('https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,400;0,600;0,700;1,400&family=Rubik&display=swap');
</style>
`;


const AnalyticsCode = 'G-7S3Y1HE297';


const { site } = useData()

const state = ref({
    fonts: false,
    analytics: false,
    isSet: false,
});
const key = 'vitepress_ls_state_' + site?.value?.title?.replace(/[^\w\d]/g, '');



onMounted(() => {

    state.value = {
        fonts: false,
        analytics: false,
        ...(JSON.parse(localStorage.getItem(key)) ?? {})
    }


    document.body.addEventListener('click', e => {
        if (e.target.closest('.VPSocialLink[aria-label*="Privacy"]')) {
            e.preventDefault();
            isOpen.value = true;
        }
    });

    if (!state.value.isSet) {
        isOpen.value = true;
    }

    if (state.value.fonts) {
        document.head.insertAdjacentHTML('beforeend', fontCSS);
    }
    if (state.value.analytics) {
        window.dataLayer = window.dataLayer || [];
        window.gtag = function () {
            window.dataLayer.push(arguments);
        }
        gtag('js', new Date());

        const s = document.createElement('script');
        s.addEventListener('load', () => {
            gtag('config', AnalyticsCode);
        });
        s.setAttribute('async', '');
        s.setAttribute('src', `https://www.googletagmanager.com/gtag/js?id=${AnalyticsCode}`);
        document.head.appendChild(s);

    }
});



const accept = function (what) {
    what = Object.fromEntries(what.map((w) => [w, true]));
    what.isSet = true;
    localStorage.setItem(key, JSON.stringify(what));
    state.value = what;
    isOpen.value = false;
    location.reload();

}



</script>
<style scoped>
#cookieConsent {
    z-index: 2222;
    position: fixed;

    .backdrop {
        position: fixed;
        bottom: 0;
        left: 0;
        right: 0;
        top: 0;
        background-color: rgba(0, 0, 0, 0.5);
    }

    .close-button {
        position: absolute;
        right: 1rem;
        top: 1rem;
    }

    .foreground {
        display: flex;
        flex-direction: column;
        position: fixed;
        bottom: 0;
        left: 0;
        right: 0;
        border: 1px solid #000;
        background-color: var(--vp-c-bg);
        padding: 2rem;
        justify-content: center;

        &>div {
            margin-bottom: 1rem;
            display: flex;
            justify-content: center;

            & a {
                text-decoration: underline;
            }
        }

        & code {
            border-radius: 4px;
            padding: 2px 2px 0px 2px;
            background-color: var(--vp-code-bg);
            transition: color 0.25s, background-color 0.5s;
            font-size: var(--vp-code-font-size);
            color: var(--vp-code-color);

        }

        & button {
            border: 1px solid #000;
            border-radius: 4px;
            padding: 3px 6px;
            background-color: var(--vp-c-bg);
            transition: color 0.25s, background-color 0.5s;
            font-size: var(--vp-code-font-size);
            color: var(--vp-code-color);
            margin: 0 0.5rem;
            cursor: pointer;



            &.accept-all {
                background-color: var(--vp-c-tip-soft);
            }

            &:hover {
                background-color: var(--vp-c-bg-hover);
                color: var(--vp-c-text-1);
            }
        }
    }
}
</style>
