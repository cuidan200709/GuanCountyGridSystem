<template>
    <div class="">
        <!---->
        <div class="PathInner" id="PathMenu" style=" margin:250px auto;">
            <div class="PathMain">
                <div class="Tmain" @click="PathRun()">
                    <div class="rotate" data-transform="rotate(0deg)"><span class="cover"></span></div>
                </div>
            </div>
            <div class="PathItem">
                <a class="link" href="#" title="污染物对比">
                    <span class="item" style="background-image:url('../../static/imgs/xzcd/曲线对比图.png'); -moz-transform: rotate(0deg);" data-transform="rotate(0deg)"></span>
                </a>
                <div class="metaicondetail shadow">
                    <div class="inner">
                        <p>这个是污染物对比模块</p>
                    </div>
                </div>
            </div>
            <div class="PathItem">
                <a class="link" href="#" title="省控点均值比">
                    <span class="item" style="background-image:url('../../static/imgs/xzcd/资料对比.png'); -moz-transform: rotate(0deg);" data-transform="rotate(0deg)"></span>
                </a>
                <div class="metaicondetail shadow">
                    <div class="inner">
                        <p>这个是省控点均值比模块</p>
                    </div>
                </div>
            </div>
            <div class="PathItem">
                <a class="link" href="#" title="固定源统计">
                    <span class="item" style="background-image:url('../../static/imgs/xzcd/统计.png'); -moz-transform: rotate(0deg);" data-transform="rotate(0deg)"></span>
                </a>
                <div class="metaicondetail shadow">
                    <div class="inner">
                        <p>这个是固定源统计</p>
                    </div>
                </div>
            </div>
            <div class="PathItem">
                <a class="link" href="#" title="乡镇空气站">
                    <span class="item" style="background-image:url('../../static/imgs/xzcd/圆心.png'); -moz-transform: rotate(0deg);" data-transform="rotate(0deg)"></span>
                </a>
                <div class="metaicondetail shadow">
                    <div class="inner">
                        <p>这个是乡镇空气站</p>
                    </div>
                </div>
            </div>
            <div class="PathItem">
                <a class="link" href="#" title="污染日历">
                    <span class="item" style="background-image:url('../../static/imgs/xzcd/日历.png'); -moz-transform: rotate(0deg);" data-transform="rotate(0deg)"></span>
                </a>
                <div class="metaicondetail shadow">
                    <div class="inner">
                        <p>这个是污染日历模块</p>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>


    export default {
        //
        name: "RevolvingMenu",
        //
        data (){
            return {
                 Radius: 160,
                 Offset : 50,
                 Path : 4,
                 OutSpeed : 80,
                 OutIncr : 50,
                 OffsetSpeed : 200,
                 InSpeed : 480,
                 InIncr : -80,
                 PathStatus : 0,
                 angle: Math.PI / ((5 - 1) * 2),
                 mainButton : [
                     {
                    'bg': '../../static/imgs/xzcd/bg-2x.png',
                    'css': '',
                    'cover': '../../static/imgs/xzcd/icon-2x.png',
                    'html': '<span class="cover"></span>'
                    },
                     {
                    'bg': '',
                    'css': '',
                    'cover': '',
                    'html': '',
                    'angle': -405,
                    'speed': 200
                }
                ],
            }
        },
        //
        mounted (){
            $(".PathItem").hover(function(){
                //
                $(this).find(".metaicondetail").show();
            },function(){
                //
                $(this).find(".metaicondetail").hide();
            });
        },
        //
        methods:{
            PathRun() {
                const _this = this;
                let PathMenu = $('#PathMenu');
                let PathItems = PathMenu.children('.PathItem').slice(0, 5);
                if (_this.PathStatus == 0) {
                    let Count = PathItems.size();
                    PathItems.each(function (SP) {
                        let ID = $(this).index();
                        if (ID == 1) {
                            var X =_this.Radius;
                            var Y = 0;
                            var X1 = X + _this.Offset;
                            var Y1 = Y;
                        } else if (ID == Count) {
                            var X = 0;
                            var Y = _this.Radius;
                            var X1 = X;
                            var Y1 = Y + _this.Offset;
                        } else {
                            var X = Math.cos(_this.angle * (ID - 1)) * _this.Radius;
                            var Y = Math.sin(_this.angle * (ID - 1)) * _this.Radius;
                            var X1 = X + _this.Offset;
                            var Y1 = Y + _this.Offset;
                        }
                        if (_this.Path == 2) {
                            Y = -Y;
                            Y1 = -Y1;
                        } else if (_this.Path == 3) {
                            X = -X;
                            Y = -Y;
                            X1 = -X1;
                            Y1 = -Y1;
                        } else if (_this.Path == 4) {
                            X = -X;
                            X1 = -X1;
                        }
                        $(this).children().children().animate({
                            rotate: 720
                        }, 600);
                        $(this).animate({
                            right: X1,
                            bottom: Y1
                        }, _this.OutSpeed + SP * _this.OutIncr, function () {
                            $(this).animate({
                                right: X,
                                bottom: Y
                            }, _this.OffsetSpeed);
                        });
                    });
                    if (_this.mainButton[1]['angle']) {
                        $(PathMenu.children('.PathMain').find('.rotate')).animate({
                            rotate: _this.mainButton[1]['angle']
                        }, _this.mainButton[1]['speed']);
                    }
                    if (_this.mainButton[1]['bg'] != '') $(this).children().css('background-image', 'url(' + _this.mainButton[1]['bg'] + ')')
                    if (_this.mainButton[1]['css'] != '') $(this).children().css(_this.mainButton[1]['css']);
                    if (_this.mainButton[1]['cover'] != '') $(this).children().children().css('background-image', 'url(' + _this.mainButton[1][
                        'cover'] + ')');
                    if (_this.mainButton[1]['html'] != '') $(this).children().html(_this.mainButton[1]['html']);
                    _this.PathStatus = 1;
                } else if (_this.PathStatus == 1) {
                    PathItems.each(function (SP) {
                        if (parseInt($(this).css('right')) == 0) {
                           var X1 = 0;
                        } else {
                            if (_this.Path <= 2) {
                                X1 = parseInt($(this).css('right')) + _this.Offset;
                            } else if (_this.Path >= 3) {
                                X1 = parseInt($(this).css('right')) - _this.Offset;
                            }
                        }
                        if (parseInt($(this).css('bottom')) == 0) {
                           var Y1 = 0;
                        } else {
                            if (_this.Path == 3 || _this.Path == 2) {
                                Y1 = parseInt($(this).css('bottom')) - _this.Offset;
                            } else if (_this.Path == 1 || _this.Path == 4) {
                                Y1 = parseInt($(this).css('bottom')) + _this.Offset;
                            }
                        }
                        $(this).children().children().animate({
                            rotate: 0
                        }, 600);
                        $(this).animate({
                            right: X1,
                            bottom: Y1
                        }, _this.OffsetSpeed, function () {
                            $(this).animate({
                                right: 0,
                                bottom: 0
                            }, _this.InSpeed + SP * _this.InIncr);
                        });
                    });
                    if (_this.mainButton[1]['angle']) {
                        $(PathMenu.children('.PathMain').find('.rotate')).animate({
                            rotate: 0
                        }, _this.mainButton[1]['speed']);
                    }
                    if (_this.mainButton[0]['bg'] != '') $(this).children().css('background-image', 'url(' + _this.mainButton[0]['bg'] + ')')
                    if (_this.mainButton[0]['css'] != '') $(this).children().css(_this.mainButton[0]['css']);
                    if (_this.mainButton[0]['cover'] != '') $(this).children().children().css('background-image', 'url(' + _this.mainButton[0][
                        'cover'] + ')');
                    if (_this.mainButton[0]['html'] != '') $(this).children().html(_this.mainButton[0]['html']);
                    _this.PathStatus = 0;
                }
            }
        }
    }
</script>

<style lang="scss" scoped>

    .PathMenu {
        position:absolute;
        right:0;
        bottom:0;
        width:60px;
        height:60px
    }
    .PathInner {
        position:relative;
        width:60px;
        height:60px
    }

    .PathInner .PathItem {
        position:absolute;
        right:0;
        bottom:0
    }
    .PathInner a {
        display:block;
        background-position:center;
        background-repeat:no-repeat;
        z-index:999
    }
    .PathInner a .item, .rotate {
        width:100%;
        height:100%;
        background-position:center;
        background-repeat:no-repeat;
        display:block;
        overflow:hidden;
        text-align:center;
        vertical-align:middle;
        position:absolute
    }
    .item{
        border-radius: 50%;
        box-shadow: 0 0 15px #666;
    }
    .PathInner .PathMain {
        z-index:1000;
        position:absolute;
        display:block;
        background-position:center;
        background-repeat:no-repeat;
        bottom:0;
        right:0
    }
    .PathInner .PathMain .Tmain {
        background-image:url("../../static/imgs/xzcd/bg-2x.png");
        width:60px;
        height:60px
    }
    .PathInner .PathMain .Tmain .rotate {
        background-image:url(../../static/imgs/xzcd/icon-2x.png);
        -moz-transform:rotate(0deg);
        position:absolute;
        bottom:0;
        right:0
    }
    .PathInner .cover {
        width:100%;
        height:100%;
        display:block;
        background:url(../../static/imgs/xzcd/bg-hl-2x.png) center no-repeat;
        cursor:pointer;
        *filter:alpha(opacity=0);
        filter:alpha(opacity=0)\0;
        opacity:0;
        -webkit-transition:opacity .2s ease-out;
        -moz-transition:opacity .2s ease-out;
        -ms-transition:opacity .2s ease-out;
        -o-transition:opacity .2s ease-out
    }
    .PathInner .cover:hover {
        opacity:.2;
        *filter:alpha(opacity=20);
        filter:alpha(opacity=20)\0
    }
    .PathItem .link {
        position:absolute;
        bottom:0;
        right:0;
        width:60px;
        height:60px;

    }
    .PathItem .link .item {
        bottom:0;
        right:0
    }
    .metaicondetail {
        background:#fff;
        color:#fff;
        border-radius:6px;
        border:1px #333 solid;
        min-width:100px;
        max-width:300px;
        z-index: 1001;
        overflow:hidden;
        text-align:center;
        position:absolute;
        display:none;
        top:-105px;
        right:-22px
    }
    .metaicondetail .inner {
        border:2px #fff solid;
        border-radius:6px;
        background:#272727;
        font-size:14px;
        padding:5px
    }
    .metaicondetail span {
        font-family:candara, arail;
        font-size:18px
    }
    .metaicondetail s {
        border-color:#333 transparent transparent;
        border-style:solid dashed dashed;
        border-width:5px;
        clear:both;
        display:inline-block;
        font-size:0;
        height:0;
        margin-right:3px;
        overflow:hidden;
        position:absolute;
        right:50%;
        top:44px;
        width:0
    }
</style>