<script setup>
defineProps({
    returnWelcomePage: Function
})
</script>

<template>
    <header>
        <div role="button" tabindex="0" @click="returnWelcomePage" class="page-title">
            <h1 class="text-gradient">FITNESS APP</h1>
        </div>
    </header>
    <main>
        <slot />
    </main>
    <footer>
        <small>Create by</small>
        <a href="https://github.com/thuhuong162075" target="_blank">
            <img alt="pfp" src="https://avatars.githubusercontent.com/u/43499837?v=4" />
            <p>Hanna</p>
            <i class="fa-brands fa-github"></i>
        </a>
    </footer>
</template>

<style scoped>

    .page-title {
        cursor: pointer;
        width: fit-content;
    }

    header, footer, main {
        padding: 1rem;
        width: 100%;
        max-width: 600px;
        margin: 0 auto;
    }

    footer {
        display: flex;
        flex-direction: column;
        gap: 0.5rem;
        align-items: center;
        padding: 2rem 0;
        padding-bottom: 3rem;
    }

    footer a {
        display: flex;
        align-items: center;
        gap: 0.5rem;
        padding: 0.3rem;
        padding-right: 0.5rem;
        background: var(--background-muted);
        border-radius: 4rem;
        border: 1px solid transparent;
        transition-duration: 200ms;
        text-decoration: none;
    }

    footer a:hover {
        border-color: var(--color-link);
    }

    footer a img {
        max-width: 30px;
        aspect-ratio: 1/1;
        border-radius: 100%;
    }
</style>
