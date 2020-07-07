<template>
    <header class="header">
        <div class="header__container">
            <a href="/" class="header__title">todooo</a>
            <Themes/>
        </div>
    </header>
</template>

<script>
import Themes from './Themes'
export default {
    name: "AppHeader",

    components: {
        Themes
    }
}
</script>

<style scoped lang="scss">
    .header {
        background: var(--theme);
        color: var(--light);
        position: absolute;
        left: 0;
        right: 0;
        top: 0;
        transition: var(--transition);

        @media (max-width: 750px) {
            padding: 0 var(--gutter);
        }
    }

    .header__container {
        align-items: center;
        display: flex;
        justify-content: space-between;
        max-width: 400px;
        margin: 0 auto;
        padding: .5rem 0;
    }

    .header__title {
        color: var(--light);
        font-weight: bold;
        text-decoration: none;
    }
</style>