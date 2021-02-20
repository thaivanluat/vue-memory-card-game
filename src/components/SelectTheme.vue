<template>
    <div class="select-theme text-center">
        <main role="main" class="inner cover">
            <h1 class="cover-heading">Select theme</h1>
        </main>
        <div class="d-flex justify-content-center">
            <button href="#" class="btn btn-lg btn-secondary"
                v-on:click="selectTheme('fruit')"
            >Fruit</button>
            <button href="#" class="btn btn-lg btn-secondary"
                v-on:click="selectTheme('girl')"
            >Girl</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SelectTheme',
    methods: {
        selectTheme(theme) {
            this.$emit('handleSelectTheme', theme);
        }
    }
}
</script>

<style scoped>
    .select-theme {
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
        width: 100%;
        height: 200px;
    }
    h1 {
        margin-bottom: 50px;
    }
    button {
        margin: 0px 10px;
    }
</style>