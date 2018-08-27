<template>
    <div class="pollution">
        <!--首页部分-->
        <v-header></v-header>
        <!--日历-->
        <div class="warp-content">
            <div class="con-cont">
                <div class="warp-coter" style="margin-top: 20px">
                    <el-radio-group v-model="valueOne" size="medium">
                        <el-radio-button label="环比" ></el-radio-button>
                        <el-radio-button label="同比"></el-radio-button>
                    </el-radio-group>
                </div>
                <div class="warp-float">
                    <el-radio-group v-model="valueTwo" size="small">
                        <el-radio-button label="AQI"></el-radio-button>
                        <el-radio-button label="PM2.5"></el-radio-button>
                        <el-radio-button label="PM10"></el-radio-button>
                        <el-radio-button label="SO2"></el-radio-button>
                        <el-radio-button label="NO2"></el-radio-button>
                        <el-radio-button label="CO"></el-radio-button>
                        <el-radio-button label="O3"></el-radio-button>
                    </el-radio-group>
                </div>
            </div>
                <!---->
            <div class="warp-calendar">
                <div id="calendar"></div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        name: "pollution",
        data(){
            return{
                valueOne: '环比',
                //
                valueTwo: 'AQI'
            }
        },
        methods:{
           //
           editEvent(event) {
               $('#event-modal input[name="event-index"]').val(event ? event.id : '');
                $('#event-modal input[name="event-name"]').val(event ? event.name : '');
                $('#event-modal input[name="event-location"]').val(event ? event.location : '');
                $('#event-modal input[name="event-start-date"]').datepicker('update', event ? event.startDate : '');
                $('#event-modal input[name="event-end-date"]').datepicker('update', event ? event.endDate : '');
                $('#event-modal').modal();
           },
          //
          deleteEvent(event) {
                var dataSource = $('#calendar').data('calendar').getDataSource();

                for(var i in dataSource) {
                    if(dataSource[i].id == event.id) {
                        dataSource.splice(i, 1);
                        break;
                    }
                }

                $('#calendar').data('calendar').setDataSource(dataSource);
            },
            //
            saveEvent() {
                var event = {
                    id: $('#event-modal input[name="event-index"]').val(),
                    name: $('#event-modal input[name="event-name"]').val(),
                    location: $('#event-modal input[name="event-location"]').val(),
                    startDate: $('#event-modal input[name="event-start-date"]').datepicker('getDate'),
                    endDate: $('#event-modal input[name="event-end-date"]').datepicker('getDate')
                }

                var dataSource = $('#calendar').data('calendar').getDataSource();

                if(event.id) {
                    for(var i in dataSource) {
                        if(dataSource[i].id == event.id) {
                            dataSource[i].name = event.name;
                            dataSource[i].location = event.location;
                            dataSource[i].startDate = event.startDate;
                            dataSource[i].endDate = event.endDate;
                        }
                    }
                }
                else
                {
                    var newId = 0;
                    for(var i in dataSource) {
                        if(dataSource[i].id > newId) {
                            newId = dataSource[i].id;
                        }
                    }

                    newId++;
                    event.id = newId;

                    dataSource.push(event);
                }

                $('#calendar').data('calendar').setDataSource(dataSource);
                $('#event-modal').modal('hide');
            },


        },
        mounted (){
//          $('#element').data('calendar').setLanguage('zh-CN');
            $(function() {
                let _this = this;
            var currentYear = new Date().getFullYear();

            $('#calendar').calendar({
                style:'background',
                enableContextMenu: true,
                enableRangeSelection: true,
                contextMenuItems:[
                    {
                        text: 'Update',
                        click: _this.editEvent
                    },
                    {
                        text: 'Delete',
                        click: _this.deleteEvent
                    }
                ],
                selectRange: function(e) {
                    _this.editEvent({ startDate: e.startDate, endDate: e.endDate });
                },
                //鼠标滑过弹窗设置
                mouseOnDay: function(e) {
                    if(e.events.length > 0) {
                        var content = '';

                        for(var i in e.events) {
                            content += '<div class="event-tooltip-content">'
                                + '<div class="event-name" style="color:' + e.events[i].color + '">' + e.events[i].name + '</div>'
                                + '<div class="event-location">' + e.events[i].location + '</div>'
                                + '</div>';
                        }
                        $(e.element).popover({
                            trigger: 'manual',
                            container: 'body',
                            html:true,
                            content: content
                        });

                        $(e.element).popover('show');
                    }
                },
                mouseOutDay: function(e) {
                    if(e.events.length > 0) {
                        $(e.element).popover('hide');
                    }
                },
                dayContextMenu: function(e) {
                    $(e.element).popover('hide');
                },
                //需要的数据格式
                dataSource: [
                    {
                        id: 0,
                        name: 'Google I/O',
                        location: 'San Francisco, CA',
                        startDate: new Date(currentYear, 4, 28),
                        endDate: new Date(currentYear, 4, 29)
                    },
                    {
                        id: 1,
                        name: 'Microsoft Convergence',
                        location: 'New Orleans, LA',
                        startDate: new Date(currentYear, 2, 16),
                        endDate: new Date(currentYear, 2, 19)
                    },
                    {
                        id: 2,
                        name: 'Microsoft Build Developer Conference',
                        location: 'San Francisco, CA',
                        startDate: new Date(currentYear, 3, 29),
                        endDate: new Date(currentYear, 4, 1)
                    },
                    {
                        id: 3,
                        name: 'Apple Special Event',
                        location: 'San Francisco, CA',
                        startDate: new Date(currentYear-1, 8, 1),
                        endDate: new Date(currentYear-1, 8, 1)
                    },
                    {
                        id: 4,
                        name: 'Apple Keynote',
                        location: 'San Francisco, CA',
                        startDate: new Date(currentYear, 8, 9),
                        endDate: new Date(currentYear, 8, 9)
                    },
                    {
                        id: 5,
                        name: 'Chrome Developer Summit',
                        location: 'Mountain View, CA',
                        startDate: new Date(currentYear-1, 10, 17),
                        endDate: new Date(currentYear-1, 10, 18)
                    },
                    {
                        id: 6,
                        name: 'F8 2015',
                        location: 'San Francisco, CA',
                        startDate: new Date(currentYear, 2, 25),
                        endDate: new Date(currentYear, 2, 26)
                    },
                    {
                        id: 7,
                        name: 'Yahoo Mobile Developer Conference',
                        location: 'New York',
                        startDate: new Date(currentYear, 7, 25),
                        endDate: new Date(currentYear, 7, 26)
                    },
                    {
                        id: 8,
                        name: 'Android Developer Conference',
                        location: 'Santa Clara, CA',
                        startDate: new Date(currentYear, 11, 1),
                        endDate: new Date(currentYear, 11, 4)
                    },
                    {
                        id: 9,
                        name: 'LA Tech Summit',
                        location: 'Los Angeles, CA',
                        startDate: new Date(currentYear, 10, 17),
                        endDate: new Date(currentYear, 10, 17)
                    },
                    {
                        id: 10,
                        name: 'shen fangda',
                        location: 'Los Angeles, CA',
                        //color:'',
                        startDate: new Date(currentYear, 1, 17),
                        endDate: new Date(currentYear, 1, 20)
                    },
                ]
            });

            $('#save-event').click(function() {
                _this.saveEvent();
            });
          })
        }
    }
</script>

<style lang="scss" scoped>
    .pollution{
        width: 100%;
        height: auto;

        .warp-content{
            width: 95%;
            height: auto;
            margin: 0 auto;
            .con-cont{
                width: 95%;
                height: 100px;
                .warp-coter{

                }
                .warp-float{
                    float: right;
                    margin-right: 10px;
                }
            }

        }

    }
</style>