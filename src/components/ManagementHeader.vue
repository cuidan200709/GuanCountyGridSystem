<template>
    <div class="header">
        <div class="navbox">
            <!--环境监测-->
            <div class="logo">
                <img src="../assets/img/logo1.png" alt="logo">
                <a style="color: #fff;font-size:21px;font-weight: 500;line-height: 56px;font-family: 'Microsoft YaHei' ">固安县环境保护网格化管理平台</a>
            </div>
            <!--<div style="color: #fff;float: left;height: 56px;line-height: 66px;margin-left: 4px">2018版</div>-->
            <ul class="bnav">
            	<div>
                    <img src="../assets/img/sy-z.png" alt="">
                    <a href="#/likewinter">首页</a>
                </div>
                <div>
                    <img src="../assets/img/tc-z.png" alt="">
                    <a style="padding: 0 10px" href="#/">退出</a>
                </div>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'header',
        data () {
            return {

            }
        },
        mounted(){

        },
        methods: {

        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
    .header {
        width: 100%;
        height: 56px;

        .navbox{
            width: 100%;
            height: 56px;
            background: #2494F2;
            position: absolute;
            top: 0;
            left: 0;
            box-shadow:9px 0px 28px rgba(36,148,242,0.53);
            .logo {
                float: left;
                width: auto;

                img {
                    width: 40px;
                    height: 40px;
                    margin-top: -9px;
                    margin-left: 20px;
                    margin-right: 2px;
                }

            }
            .bnav {
                float: right;
                height: 56px;
                margin-right: 20px;
                div {
                    float: left;
                    width: auto;
                    height: 56px;
                    list-style: none;
                    line-height: 56px;
                    position: relative;
                    margin-left:20px;
                    text-align: left;
                    img{
                        display: inline-block;
                        vertical-align: middle;
                    }
                    a {
                        color: #dee9f5;
                        font-size: 16px;
                        font-family: '微软雅黑';
                        cursor: pointer;
                        padding: 0;
                    }
                    :hover {
                        text-decoration: none;
                    }
               }
            }
        }
    }

</style>
