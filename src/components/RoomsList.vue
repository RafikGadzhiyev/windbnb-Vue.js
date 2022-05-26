<template>
    <ul class = 'stays-list'>
        <StayComponent v-for="stay in stays" v-bind:key="stay"  v-bind:stayData = "stay"/>
       
    </ul>
</template>

<script>
    import StayComponent from './Room.vue';

    export default {
        name: "StaysListComponent",
        components: {
            StayComponent
        },
        props: ["stays"]
    }
</script>

<style>

.stays-list{
    list-style: none;
    display: flex;
    flex-wrap: wrap;
    gap: 25px;
    justify-content: center;    
}

</style>