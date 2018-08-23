<template>
    <div class="foghazeforecast">
        <!--雾霾预报-->
        <v-header></v-header>
       <!---->
        <h1>雾霾预报正在开发中</h1>
    </div>
</template>

<script>
    //import GridMap from '@/map/GridMapMain'

    export default {
        name: 'foghazeforecast',
        data() {
            return {
                fullscreenLoading: false
            }
        },
        mounted() {
            this.openFullScreen()

        },
        methods: {
            //加载动画
            openFullScreen() {
                this.fullscreenLoading = true;
                setTimeout(() => {
                    this.fullscreenLoading = false;
                }, 2000);
            }
        },
        components: {

        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
    .foghazeforecast {
        width: 100%;
        height: 100%;
    }
</style>
