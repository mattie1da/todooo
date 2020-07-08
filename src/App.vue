<template>
  <div id="app" :class="[`app app--${theme}`, { 'app--dark' : dark == true }]">
    <div class="todos">
      <AppHeader/>
      <Title/>
      <Tasks/>
    </div>
    <Signature/>
  </div>
</template>

<script>
import Tasks from './components/Tasks.vue'
import Title from './components/Title.vue'
import AppHeader from './components/AppHeader.vue'
import Signature from './components/Signature.vue'
import { bus } from './main.js'

export default {
  name: 'App',

  components: {
    AppHeader,
    Title,
    Tasks,
    Signature
  },

  data() {
    return {
        theme: 'black',
        dark: false,
    }
  },

  watch: {
      theme: {
          handler() {
              localStorage.setItem('theme', this.theme);
          },
          deep: true
      },
      dark: {
          handler() {
              localStorage.setItem('dark', this.dark);
          },
          deep: true
      }
  },

  created() {
      bus.$on('changeTheme', (data) => {
          this.theme = data;
      }),

      bus.$on('toggleDark', (data) => {
          this.dark = data;
      })
  },

  mounted() {
     if (localStorage.getItem('theme')) {
         this.theme = localStorage.getItem('theme');
     }

     if (localStorage.getItem('dark')) {
         this.dark = JSON.parse(localStorage.getItem('dark'));
     }
  }
}
</script>

<style lang="scss">
  :root {
    --dark: #000913;
    --light: white;
    --grey: #b2b2b2;
    --greylight: #cecece;
    --progress: #8bffae;
    --error: #ffa893;

    --red: red;
    --orange: #fbb500;
    --yellow: #fff95d;
    --blue: #004bff;
    --purple: #a200ff;
    --slushy: linear-gradient(90deg, #ff898b, #ff9dec, #ceff89, #80ebff, pink);

    --transition: all 0.2s ease-in-out;
    --gutter: 8vw;
  }

  body {
    font-family: 'Poppins', sans-serif;
    color: var(--dark);
    background: var(--light);
    margin: 0;
    padding: 0;
  }

  ::placeholder {
    color: var(--greylight);
  }

  .app {
    align-items: center;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    position: relative;
    transition: var(--transition);

    @media (max-width: 750px) {
      padding: 0 var(--gutter);
    }

    &--red {
      --theme: var(--red);
    }

    &--orange {
      --theme: var(--orange);
    }

    &--yellow {
      --theme: var(--yellow);
    }

    &--blue {
      --theme: var(--blue);
    }

    &--purple {
      --theme: var(--purple);
    }

    &--slushy {
      --theme: var(--slushy);
    }

    &--black {
      --theme: var(--dark);
    }

    &--white {
      --theme: var(--light);
    }

    &--dark {
      background: var(--dark);
      color: var(--light);

      .task__icon--checkbox {
        background: var(--dark);
        border-color: var(--light);
      }
    }

    // all cases where header title is too light for bg
    &--yellow,
    &--white,
    &--slushy {
      .header__title {
        color: var(--dark);
      }
    }
  }

  .todos {
    padding-top: 10vw;
    width: 400px;

    @media (max-width: 750px) {
      padding: 25vw 0;
      max-width: 100%;
    }
  }
</style>
