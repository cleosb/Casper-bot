<template>
    <v-app-bar
        class="light-blue lighten-5"
        app
    >
        <v-toolbar-title
            class="mr-3"
            style="cursor: pointer"
            @click="goToHomeView"
        >
            Notícias
        </v-toolbar-title>
        <v-spacer />
    </v-app-bar>
</template>

<script>
export default {
    name: "NavbarComponent",
    methods: {
        goToHomeView: function() {
            if (this.$router.currentRoute.name !== "Home") {
                this.$router.push({name: "Home"})
            }
        }
    }
}
</script>

<style>

</style>