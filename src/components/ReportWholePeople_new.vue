<template>
    <div class="Report-people">
        <!--全民举报-->
       <div class="main">
                    <!--选项查询-->
                    <div class="first">
                        <div class="shijian">

                            <el-input v-model="inputName" placeholder="请输入内容"></el-input>

                        </div>
                        <div class="btnns">
                            <button @click="btnClickEvent">查询</button>
                        </div>
                    </div>
                    <!--图表-->
                    <div class="chars-tubiao">
                        <!--案件网格数量占比-->
                        <div class="title-daqi">
                            <div class="shuxian"></div>
                            <div class="title-text">
                                <div class="bing_text">状态</div>
                            </div>
                        </div>
                        <div class="">
                            <div id="bing_item_1p" style="width: 390px;height: 180px;padding-left: 14px"></div>
                        </div>
                        <!--&lt;!&ndash;&ndash;&gt;-->
                        <!--<div class="title-daqi">-->
                            <!--<div class="shuxian"></div>-->
                            <!--<div class="title-text">-->
                                <!--<div class="bing_text">乡镇名称</div>-->
                            <!--</div>-->
                        <!--</div>-->
                        <!--&lt;!&ndash;案件类型占比&ndash;&gt;-->
                        <!--<div class="">-->
                            <!--<div id="bing_item_2p" style="width: 390px;height: 180px;padding-left: 14px"></div>-->
                        <!--</div>-->
                    </div>
                    <!--排名-->
                    <div class="table_container">
                        <el-table
                                :data="tableData"
                                border
                                stripe
                                highlight-current-row
                                @current-change="RowCurrentChange"
                                style="width: 400px">
                            <el-table-column
                                    property="name"
                                    label="姓名"

                                    >
                            </el-table-column>
                            <el-table-column
                                    property="Contact"
                                    label="联系方式"
                                    >
                            </el-table-column>
                            <el-table-column
                                    property="state"
                                    label="状态"
                                    >
                            </el-table-column>
                            <el-table-column
                                    label="操作"
                                   >
                                <template scope="scope">
                                    <el-button @click="handleClick(scope.row)" type="text" size="small" class='eidt'>调度</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <!--分页条-->
                        <div class="Pagination" style="text-align: left;margin-top: 10px;">
                            <el-pagination
                                    background
                                    @size-change="handleSizeChange"
                                    @current-change="handleCurrentChange"
                                    :current-page="currentPage"
                                    :page-size="pagesize"
                                    layout="total, prev, pager, next"
                                    :total="totalCount">
                            </el-pagination>
                        </div>
                        <div class="pan-anniu">
                            <el-button type="primary" @click="OnkeyScheduling">一键调度</el-button>
                        </div>
                    </div>
                </div>
    </div>
</template>

<script>
    import {bus} from '@/js/bus.js'
    import api from '../api/index'

    export default {
        name: 'Reportpeople',
        data() {
            return {
                inputName:'',
                //列表数据
                tableData: [],
                //存储分页数据
                idsdata: [],
                //
                currentRow: null,
                //分页每页
                pagesize: 10,
                //当前页码
                currentPage: 1,
                //分页总数量
                totalCount: 10,
                //开始时间控制区
                pickerOptions0: {},
                //结束时间控制区
                pickerOptions1: {},
                //开始时间
                starttime: '',
                //结束时间
                endtime: '',
            }
        },
        created() {},
        mounted() {
            //
            this.GetListData();
            //
            this.GetChartRtdata();
        },
        methods: {
            //调度单个
            handleClick(val){
                console.log(val)
                let userId = val.id;
                let title ='';
                let message = '';
                let sendId ='';
                api.PostSendSchduleRt(userId,title,message,sendId).then(res =>{
                    console.log(res);
                })
            },
            //排序
            compare(propertyName) {
                return function (object1, object2) {
                    let value1 = object1[propertyName]
                    let value2 = object2[propertyName]
                    return value2 - value1
                }
            },
            //初数据始化列表
            InitializationDataMethod(data) {
                const _this = this;
                //
                _this.tableData = [];
                _this.idsdata = [];
                //
                data.forEach(item => {
                    const tableData = {};
                    tableData.id = item.Id;//城市id
                    tableData.name = item.name;//
                    tableData.Contact = item.mobile;//状态
                    tableData.state = item.status?'开启':'关闭';//状态
                    //tableData.latitude = item.latitude;//纬度
                    //tableData.longitude = item.longitude;//经度
                    _this.tableData.push(tableData);
                    _this.idsdata.push(item.Id);
                })
                // console.log(this.tableData)
            },
            //table点击事件
            RowCurrentChange(val) {
                this.currentRow = val;
                //地图联动
                //bus.$emit('locationClick', 'layer_gd', this.currentRow ,this.type);
            },
            //每页显示数据量变更
            handleSizeChange(val) {
                //
                this.pagesize = val;
            },
            //页码变更
            handleCurrentChange(val) {
                //
                let name = this.inputName;
                let PageIndex = val;
                this.GetListData(name,PageIndex)
            },
            //分页效果
            setPageTable(pageSize, pageNum) {
                let rtValue = [];
                let startNum = pageSize * (pageNum - 1);
                for (let i = 0; i < pageSize; i++) {
                    if ((startNum + i + 1) > this.alldata.length)
                        break;
                    rtValue.push(this.alldata[startNum + i]);
                }
                this.tableData = rtValue;
            },
            //数据筛选
            getPointByType(type) {
                let rtValue = [];
                let dt = this.data;
                if (dt) {
                    for (let i = 0, length = dt.length; i < length; i++) {
                        let item = dt[i];
                        if (item.type === type) {
                            rtValue.push(dt[i]);
                        }
                    }
                }
                return rtValue;
            },
            //查询事件
            btnClickEvent() {
                //
                let name = this.inputName;
                this.GetChartRtdata(name);
                this.GetListData(name,1)
            },
            //时间转换
            TimeConversion(fmt, date) {
                var o = {
                    "M+": date.getMonth() + 1,                 //月份
                    "d+": date.getDate(),                    //日
                    "h+": date.getHours(),                   //小时
                    "m+": date.getMinutes(),                 //分
                    "s+": date.getSeconds(),                 //秒
                    "q+": Math.floor((date.getMonth() + 3) / 3), //季度
                    "S": date.getMilliseconds()             //毫秒
                };
                if (/(y+)/.test(fmt))
                    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + "").substr(4 - RegExp.$1.length));
                for (var k in o)
                    if (new RegExp("(" + k + ")").test(fmt))
                        fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
                return fmt;
            },
            //状态
            NumberCasesChars(data){
                let showData = data.seriesData.data.map(function (v) {
                    return {value: v.value, name: v.name}
                }) || [];
                let lenData = data.legend.map(function (v) {
                    return v
                }) || [];
                // 基于准备好的dom，初始化echarts实例
                let myChart = echarts.init(document.getElementById('bing_item_1p'));
                // 指定图表的配置项和数据
                let option = {

                    legend: {
                        orient: 'right',
                        y:'center',
                        left:160,
                       // data: ['广阳区','安次区','开发区']
                        data:lenData
                    },
                    series: [
                        {
                            name: '访问来源',
                            type: 'pie',
                            radius: '70%',
                            center: ['20%', '50%'],
                            hoverAnimation: false,
                            label: {
                                normal: {
                                    show: false,
                                    position: 'center'
                                },
                                emphasis: {
                                    show: true,
                                    textStyle: {
                                        fontSize: '20',
                                        fontWeight: 'bold'
                                    }
                                }
                            },
                            data: [
                                {value: 335, name: '广阳区'},
                                {value: 310, name: '安次区'},
                                {value: 234, name: '开发区'},

                            ],

                        }
                    ]
                };
                // 使用刚指定的配置项和数据显示图表。
                myChart.setOption(option);
                //动态设置参数
                myChart.setOption({
                    series: [{
                        data: showData,
                        color: [
                            '#08a1ed',
                            '#a2c73b',
                            '#f2cd49',
                            '#85dbce',
                            '#ce93e3',
                            '#6c68e1',
                            '#e5763f'
                        ]
                    }]
                })
            },
            //乡镇名称
            // TypeCasesChars(data){
            //     // let showData = data.map(function (v) {
            //     //     return {value: (v.percent).replace('%',''), name: v.name}
            //     // }) || [];
            //     // let lenData = data.map(function (v) {
            //     //     return {name: v.name}
            //     // }) || [];
            //     // 基于准备好的dom，初始化echarts实例
            //     let myChart = echarts.init(document.getElementById('bing_item_2p'));
            //     // 指定图表的配置项和数据
            //     let option = {
            //
            //         legend: {
            //             orient: 'right',
            //             y:'center',
            //             left: 'right',
            //             data: ['垃圾堆放','土壤裸露','露天烧烤','垃圾焚烧','餐饮油烟','建筑扬尘','废气排放','汽车尾气']
            //             //data:lenData
            //         },
            //         series: [
            //             {
            //                 name: '访问来源',
            //                 type: 'pie',
            //                 radius: '70%',
            //                 center: ['20%', '50%'],
            //                 hoverAnimation: false,
            //                 label: {
            //                     normal: {
            //                         show: false,
            //                         position: 'center'
            //                     },
            //                     emphasis: {
            //                         show: true,
            //                         textStyle: {
            //                             fontSize: '20',
            //                             fontWeight: 'bold'
            //                         }
            //                     }
            //                 },
            //                 data: [
            //                     {value: 335, name: '垃圾堆放'},
            //                     {value: 310, name: '土壤裸露'},
            //                     {value: 234, name: '露天烧烤'},
            //                     {value: 135, name: '垃圾焚烧'},
            //                     {value: 548, name: '餐饮油烟'},
            //                     {value: 500, name: '建筑扬尘'},
            //                     {value: 500, name: '废气排放'},
            //                     {value: 300, name: '汽车尾气'}
            //                 ],
            //
            //             }
            //         ]
            //     };
            //     // 使用刚指定的配置项和数据显示图表。
            //     myChart.setOption(option);
            //     //动态设置参数
            //     myChart.setOption({
            //         series: [{
            //            // data: showData,
            //             color: [
            //                 '#08a1ed',
            //                 '#a2c73b',
            //                 '#f2cd49',
            //                 '#85dbce',
            //                 '#ce93e3',
            //                 '#6c68e1',
            //                 '#e5763f'
            //             ]
            //         }]
            //     })
            //     //触动当前方法
            //     myChart.on('legendselectchanged', function(obj) {
            //         let selected = obj.selected;
            //         let legend = obj.name;
            //         if(selected[legend] == false){
            //             console.log(legend);
            //         }
            //     });
            // },
            //获取饼图数据
            GetChartRtdata(name = ''){
                //饼图数据
                api.GetInspectorChartRt(name).then(res =>{
                    console.log(res)
                    let data = res.data.Data;
                    this.NumberCasesChars(data);
                })
            },
            //获取列表数据
            GetListData(name = '',PageIndex = 1){

                api.PostSchduleListRt(name,PageIndex).then(res =>{
                   // console.log(res)
                    let TotlePageNum = res.data.Data.TotlePageNum;
                    let data = res.data.Data.Data;
                    this.totalCount = TotlePageNum;
                    this.InitializationDataMethod(data);
                })
            },
            //一键调度
            OnkeyScheduling(){
                let userId = this.idsdata;
                let title ='';
                let message = '';
                let sendId ='';
                api.PostSendSchduleRt(userId,title,message,sendId).then(res =>{
                    console.log(res);
                })
            }
        },
        components: {}
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

    .Report-people {
        width: 430px;
        height: auto;
        /**/
        .title-daqi{
            width: 100%;
            height: 36px;
            .shuxian{
                width: 3px;
                height: 24px;
                background: #1FA1FF;
                float: left;
                margin-left: 10px;
            }
            .title-text{
                float: left;
                font-weight: bold;
                line-height: 24px;
                padding-left: 10px;
                text-align: left;
            }
        }
        img {
                position: absolute;
                top: 40%;
                left: -17px;
                cursor: pointer;
                float: left;
            }
        .main {
                height: auto;
                .pan-anniu{
                    width: 100%;
                    height: 40px;
                    text-align: right;
                    padding: 10px 20px;
                }
                .kbiaoti {
                    padding: 10px 0px;
                }

                .bluexian {
                    width: 26px;
                    border: 1px solid #1080cc;
                    margin: 0 auto;
                }

                .first {
                    width: 100%;
                    height: 44px;
                    margin-top: 30px;

                    .tables {
                        float: left;
                        margin-left: 14px;

                        a {
                            padding: 0 15px;
                            float: left;
                            text-decoration: none;
                            color: #666;
                            display: inline-block;
                            line-height: 34px;
                            width: 100px;
                            height: 34px;
                            margin-right: 16px;
                            border: solid 1px #ccc;
                            background: #fff;
                            border-radius: 2px;
                        }

                    }
                    .shijian {
                        float: left;
                        margin-left: 10px;
                        margin-right: 20px;
                    }

                    .btnns {
                        float: left;
                        margin-left: 6px;

                        button {
                            border: none;
                            width: 80px;
                            height: 34px;
                            background: #1FA1FF;
                            color: #fff;
                            border-radius: 4px;
                        }

                    }
                }
                .tqbiaoti {
                    width: 100%;
                    height: 22px;
                    padding-left: 20px;
                    text-align: left;
                    margin-left: 16px;
                    border-left: solid 3px #2a6496;
                }

                .table_container {
                    margin-left: 16px;
                }

            }
    }

</style>
