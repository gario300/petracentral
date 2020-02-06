<template>
    <div>
        <GmapMap v-if="lat !== null || lng !== null && image !== null"
        :center="{lat: parseFloat(lat), lng: parseFloat(lng)}"
        :zoom="18"
        map-type-id="terrain"
        class="nofull"
        :class="{'full':full}"
        >
        <GmapMarker
            :position="{lat: parseFloat(lat), lng: parseFloat(lng)}"
            :clickable="true"
            :draggable="true"
            @click="center=m.position"
        />
        </GmapMap>
        <img 
        v-if="lat == null && lng == null 
        || lat == null && lng !== null || lng ==null && lat !== null
        && image == null"
        src="https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcTd2Tu-WhrNbxIAo_2OWwgzwmhBASgkPUv9FAHRhWy3Ls2icMOz"
        class="nofull"
        :class="{'full':full}"
        >
        <img 
        v-if="lat == null && lng == null && image !== null
        || lat == null && lng !== null || lng ==null && lat !== null"
        :src="image"
        class="nofull"
        :class="{'full':full}">
    </div>
</template>

<script>
    export default {
        name: 'gmap',
        props: {
            lat: String,
            lng: String,
            full: Boolean,
            image: String
        },
        data(){
            return{

            }
        },
        created(){

        },
        methods:{

        }
    }
</script>

<style scoped>
.nofull{
width: 450px; 
height: 250px
}
.full{
    width: 100%;
    height: 500px;
}

</style>