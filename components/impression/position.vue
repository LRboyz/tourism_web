<template>
    <div>
        position components
    </div>
</template>

<script>
// import BaiduMap from 'vue-baidu-map/components/map/Map.vue'

export default {
    name: 'Position',
    mounted() {
        // this.initMap()
    },
    data() {
        return {
            center: { lng: 109, lat: 30 },
            zoom: 3
        }
    },
    components: {
        // BaiduMap
    },
    methods: {
        // handler({ BMap, map }) {
        //     console.log(BMap, map)
        //     this.center.lng = 109
        //     this.center.lat = 30
        //     this.zoom = 15
        // }
    }
}
</script>

<style>
.bm-view {
    margin-top: 20px;
    width: 100%;
    height: 800px;
}
</style>
