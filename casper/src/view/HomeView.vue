<template>
    <v-container
    fill-height
    fluid
    >
        <v-row justify="center">
            <v-col>
                <admin-dialog-component />
                <noticia-table-component />
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
import AdminDialogComponent from "@/components/AdminDialogComponent.vue";
import NoticiaTableComponent from '@/components/NoticiaTableComponent.vue';
export default {
    name: "HomeView",
    components: {
        AdminDialogComponent,
        NoticiaTableComponent
    }
}
</script>

<style>

</style>