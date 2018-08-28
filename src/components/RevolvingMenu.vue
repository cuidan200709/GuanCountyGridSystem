<template>
    <div class="revolvingmenu">
        <div class="testfield">
            <div class="flyout-wrap">
                <!---->
                <ul class="flyout flyout-init">
                    <li style="z-index: 1000"><a class="fly-btn"><span class=""></span></a></li>
                    <li style="z-index: 998"><a @click="chuandiclick('WRWDB')" title="污染物对比"><span class="icon-wrwdb"></span></a></li>
                    <li style="z-index: 996"><a @click="chuandiclick('SKJZB')" title="省控点均值比"><span class="icon-sjzb"></span></a></li>
                    <li style="z-index: 994"><a @click="chuandiclick('GDYTJ')" title="固定源统计"><span class="icon-gdytj"></span></a></li>
                    <li style="z-index: 992"><a @click="chuandiclick('XZKQZ')" title="乡镇空气站"><span class="icon-xzkqz"></span></a></li>
                    <li style="z-index: 990"><a href="#/pollution" title="污染日历"><span class="icon-wrrl"></span></a></li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>

    import {bus} from '@/js/bus.js'
    export default {
        //
        name: "RevolvingMenu",
        //
        data (){
            return {

            }
        },
        //
        mounted (){
                $(".fly-btn").click(function() {
                    $(".flyout li").toggleClass("flyoutposition");
                });
        },
        //
        methods:{
            chuandiclick(data){
               let dav = data;
               console.log(dav);
               bus.$emit('navactive',dav);
                //阻止冒泡
                return false;
            }
        }
    }
</script>

<style lang="scss" scoped>

    .revolvingmenu{
        width: 100%;
        height: 40px;
        .flyoutposition{
            position: absolute;
            transform: rotateY(180deg);
        }
        /*0001*/
        .testfield{
            .flyout-wrap{
                width: 440px;
                height: auto;
                position: relative;

                .flyout{
                    width: 335px;
                    height: auto;
                    float: left;

                    li{
                        transition: all 500ms;
                        width: 40px;
                        height: 40px;
                        float: left;
                        margin-left: 15px;
                        border-radius: 10px;
                        list-style: none;
                        background: #1080cc;
                        a{
                            display: inline-block;
                            width: 100%;
                            height: 100%;
                        }
                    }

                }
            }
        }
    }
</style>
