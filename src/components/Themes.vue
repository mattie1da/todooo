<template>
    <div class="themes">
        <a href="#" class="theme" @click="showThemes"></a>

        <aside v-if="open" class="palette">
            <a
                href="#"
                :key="theme.index"
                v-for="(theme, index) in themes"
                :class="[`theme theme--${theme}`, { 'theme--active' : activeTheme == index }]"
                @click="selectTheme(theme)"
            >
            </a>
            <a
                href="#"
                class="mode"
                :class="{ 'mode--dark' : dark === true }"
                @click="toggleDark()"
            >
            </a>
        </aside>

    </div>
</template>

<script>
import { bus } from '../main.js'

export default {
    name: "Themes",

    data() {
        return {
            open: false,
            dark: false,
            activeTheme: null,

            themes: [
                'red',
                'orange',
                'yellow',
                'blue',
                'purple',
                'slushy',
                'black',
                'white'
            ],
        }
    },

    methods: {
        showThemes() {
            this.open = !this.open;
        },

        selectTheme(theme) {
            this.activeTheme = this.themes.indexOf(theme);
            bus.$emit('changeTheme', theme);
        },

        toggleDark() {
            this.dark = !this.dark;
            bus.$emit('toggleDark', this.dark);
        }
    },

    mounted() {
        if (localStorage.getItem('dark')) {
            this.dark = JSON.parse(localStorage.getItem('dark'));
        }
    }
}
</script>

<style scoped lang="scss">
    .theme {
        border: 2px solid var(--light);
        border-radius: 50%;
        display: block;
        height: .75rem;
        width: .75rem;
        text-decoration: none;

        &--red {
            background: var(--red);
            border-color: var(--red);
        }

        &--orange {
            background: var(--orange);
            border-color: var(--orange);
        }

        &--yellow {
            background: var(--yellow);
            border-color: var(--yellow);
        }

        &--blue {
            background: var(--blue);
            border-color: var(--blue);
        }

        &--purple {
            background: var(--purple);
            border-color: var(--purple);
        }

        &--slushy {
            background: var(--slushy);
            border-color: transparent;
        }

        &--black {
            background: var(--dark);
            border-color: var(--dark);
        }

        &--white {
            background: var(--light);

            &.theme--active {
                border-color: var(--dark);
            }
        }

        &--active {
            border-color: var(--light);
        }

        .app--white .themes > & { border-color: var(--dark); }
        .app--yellow .themes > & { border-color: var(--dark); }
        .app--slushy .themes > & { border-color: var(--dark); }
    }

    .mode {
        @extend .theme;
        border: 2px solid transparent;
        position: relative;

        &:after {
            content: '🌚';
            left: -3px;
            position: absolute;
            top: -7px;
        }

        &--dark {
            &:after {
                content: '🌝';
                left: -3px;
                position: absolute;
                top: -7px;
            }
        }
    }

    .themes {
        position: relative;
    }

    .palette {
        background: rgba(lightgray, 0.5);
        border-radius: 1rem 0 1rem 1rem;
        display: grid;
        grid-gap: 1rem;
        grid-template-columns: repeat(3, auto);
        padding: 1rem;
        position: absolute;
        right: 0;
        top: calc(100% + 1rem);
    }
</style>