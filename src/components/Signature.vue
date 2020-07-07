<template>
    <footer class="signature">
        <p>by <a href="https://mattie.world" target="_blank">mattie1da</a></p>
    </footer>
</template>

<script>
export default {
    name: "Signature"
}
</script>

<style scoped lang="scss">
    .signature {
        bottom: 2vw;
        display: flex;
        font-size: .5rem;
        opacity: 0.5;
        position: absolute;
        right: 2vw;
        text-align: right;

        p, a {
            color: var(--light);
            margin: 0;
        }
    }
</style>