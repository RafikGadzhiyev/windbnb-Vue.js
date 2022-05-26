<template>
    <main class="main_content-container">
        <div class="content_theme-container">
            <h2 class="theme-title">Stays in Finland</h2>
            <span class = 'total'>{{formatStays(stays.length)}}</span>
        </div>
        <StaysListComponent v-bind:stays = "stays" />
    </main>
</template>

<script>
    import StaysListComponent from './RoomsList.vue';

    export default {
        name: "MainContentComponent",
        components: {
            StaysListComponent
        },
        props: ["stays"],
        methods:{
            formatStays(totalStays){
                return `${totalStays} stay${totalStays > 1 ? 's' : ''}`
            }
        }
    }
</script>

<style>

.content_theme-container{
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-block: 10px;
    padding-right: 30px;
}

.main_content-container{
    margin-bottom: 4rem;
}

</style>
