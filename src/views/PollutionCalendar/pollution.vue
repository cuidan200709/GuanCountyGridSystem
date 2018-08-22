<template>
    <div class="pollution">
        <!--首页部分-->
        <v-header></v-header>
        <!--日历-->
        <div class="warp-content">
            <div class="" style="margin-top: 20px">
                <el-checkbox-group v-model="checkboxGroup1">
                    <el-checkbox-button v-for="city in topcities" :label="city" :key="city">{{city}}</el-checkbox-button>
                </el-checkbox-group>
            </div>
            <div class="">
                <el-checkbox-group v-model="checkboxGroup3" size="small">
                    <el-checkbox-button v-for="city in cities" :label="city" :disabled="city === '北京'" :key="city">{{city}}</el-checkbox-button>
                </el-checkbox-group>
            </div>
            <div class="">

            </div>
            <div class="">

            </div>
        </div>
    </div>
</template>

<script>
    const cityOptions = ['上海', '北京', '广州', '深圳'];
    const topOptions = ['环比','同比'];
    export default {
        name: "pollution",
        data(){
            return{
                checkboxGroup1: ['环比'],
                //
                checkboxGroup3: ['上海'],
                //
                cities: cityOptions,
                //
                topcities: topOptions
            }
        },
    }
</script>

<style lang="scss" scoped>
    .pollution{
        width: 100%;
        height: auto;
        .warp-content{
            width: 95%;
            height: auto;
        }
    }
</style>