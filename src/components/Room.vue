<template>
    <li class = 'stay'>
        <img :src="stayData.photo" alt="stay picture" class = 'stay_image' loading = "lazy">
         <div class="stay_details-container">
            <span class="super_stay" v-bind:class="{active: stayData.superHost}" >super host</span>
            <span class="stay-data">
                <span class="stay-type">
                    {{stayData.type}}. 
                </span>
                <span class="total-bed">
                    {{formatStayData(stayData.beds)}}. 
                </span>
                <span class="total-guests">{{formatGuests(stayData.maxGuests)}}.</span>
            </span>
            <span class="stay-rating">
                <i class="bi bi-star-fill rating_icon"></i>
                {{stayData.rating}}
            </span>
        </div>
        <span class="stay_description" :title="stayData.title">
            {{formatString(stayData.title, 36)}}
        </span>
    </li>
</template>


<script>
    export default {
        name: "StayComponent",
        props: ['stayData'],
        methods: {
            formatString(string, maxLength){
                if(string.length < maxLength) return string;

                return string.slice(0, maxLength) + '...';
            },
            formatStayData(bedValue){
                return `${bedValue || 0} bed${bedValue > 1 ? 's' : ''}`
            },
            formatGuests(total){
                return `${total} guest${total > 1 ? 's' : ''}`
            }
        }
    }

</script>

<style>
    .stay{
        width: 395px;
        box-shadow: 1px 1px 10px -4px #000;
        border-radius: 10px;
        padding-bottom: 10px;
    }
    .stay_image{
        width: 100%;
        aspect-ratio: 1/.75;
        border-radius: 10px 10px 0 0 ;
        object-fit: cover;
    }

    .super_stay{
        display: none;
    }

    .super_stay.active{
        display: inline-block;
        text-transform: uppercase;
        color: black;
        border-radius: 10px;
        border: 1px solid;
        font-size: 10px;
        padding: 2.5px 5px;
        font-weight: 700;
    }

    .stay_details-container{
        display: flex;
    justify-content: space-between;
    padding-block: 10px;
        padding-inline: 10px;
    }
    .stay_description{
        font-weight: 700; 
        padding-inline: 10px;

    }

    .rating_icon{
        color: #EB5757;
    }
    .stay-data{
        color: #828282;

    }

    @media screen and (max-width: 360px){

        .stay_details-container{
            gap: 10px;
        }

        .super_stay.active{
            display: block;
            width: 29%;
            height:fit-content;
        }
    }

</style>