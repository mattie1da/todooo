<template>
    <div>
        <h1 class="title">good {{ welcome }},<br>here's your todo today:</h1>
    </div>
</template>

<script>
export default {
    name: "Title",

    computed: {
        welcome() {
            let time = new Date().getHours();

            if ( time <= 12 ) {
                return 'morning'
            } else if ( time > 12 && time < 19) {
                return 'afternoon'
            } else {
                return 'evening'
            }
        }
    }
}
</script>

<style scoped>
    .title {
        line-height: 1.2;
    }
</style>