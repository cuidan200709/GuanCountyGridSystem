
<!--后台管理-巡查员管理-->
<template>
    <div class="businessOperation">
		<!--------------右侧数据展示------>
		<div id="right">
			<div class="box">
                <div class="warning">
                    <a>巡查员管理</a>
                </div>
            </div>
            <!-----------查询部分------->
			<div class="search">
				<span>网格员名称</span><el-input v-model="input" placeholder="请输入内容"></el-input>
				<span>级别</span>
				<el-select v-model="levelVal" placeholder="请选择">
				    <el-option
				      v-for="item in optionsLevelVal"
				      :key="item.value"
				      :label="item.label"
				      :value="item.value">
				    </el-option>
				</el-select>
				<el-button type="primary" class='btns' @click="">查询</el-button>
				<el-button type="primary" class='btns' @click="openWin">添加网格员</el-button>
				
			</div>
			<div class="search">
				<span>组织</span>
				<el-select v-model="organizeVal" placeholder="请选择">
				    <el-option
				      v-for="item in optionsOrganizeVal"
				      :key="item.value"
				      :label="item.label"
				      :value="item.value">
				    </el-option>
				</el-select>
				<span>岗位</span>
				<el-select v-model="stationVal" placeholder="请选择">
				    <el-option
				      v-for="item in optionsStationVal"
				      :key="item.value"
				      :label="item.label"
				      :value="item.value">
				    </el-option>
				</el-select>
				<el-button type="primary" class='btns' @click="">导出</el-button>
			</div>
			
			<!--------------列表部分---------->
			<div class="box">
                <div class="warning">
                    <a>列表</a>
                </div>
            </div>
            <el-table
			    :data="tableData"
			    style="width: 100%">
			    <el-table-column
			      prop="DeviceName"
			      label="登录名"
			      width="100">
			    </el-table-column>
			    <el-table-column
			      prop="CreateTime"
			      label="姓名"
			      width="100">
			    </el-table-column>
			    <el-table-column
			      prop="CreateTime"
			      label="组织"
			      width="200">
			    </el-table-column>
			    <el-table-column
			      prop="CreateTime"
			      label="岗位"
			      width="200">
			    </el-table-column>
			    <el-table-column
			      prop="CreateTime"
			      label="登录APP"
			      width="200">
			    </el-table-column>
			    <el-table-column
			      prop="CreateTime"
			      label="处理案件"
			      width="200">
			    </el-table-column>
			    <el-table-column
			      label="操作"
			      width="">
			      <template scope="scope">
			        <el-button @click="handleClick(scope.row)" type="text" size="small" class='eidt'>编辑</el-button>
			        <span style="color: #000;">|</span>
			        <el-button @click="handleClick(scope.row)" type="text" size="small" class='eidt'>重置密码</el-button>
			        <span style="color: #000;">|</span>
			        <el-button @click="handleClick(scope.row)" type="text" size="small" class='eidt'>修改密码</el-button>
			        <span style="color: #000;">|</span>
			        <span class="OverBox">
			        	<el-button @click="DeleteOperatorInfo(scope.row)" type="text" size="small" class='eidt'>删除</el-button>
			        </span>
			     
			      </template>
			    </el-table-column>
			</el-table>
		   	<div class="page" style="">
			    <span class="demonstration">共找到{{totalCount}}条记录</span>
			    <el-pagination
			      @size-change="handleSizeChange"
			      @current-change="handleCurrentChange"
			      :current-page="currentPage"
			      :page-size="pagesize"
			      layout="prev, pager, next, jumper"
			      :total="totalCount">
			    </el-pagination>
			</div>
			<!--------------添加弹框部分--------------->
			<div class="popUp" v-if="isNew">
	            <div class="mask"></div>
	            <div class="succ-pop">
	                <div class="title">
	                    <a id="newCreate">添加</a>
	                    <div class="el-icon-close" @click="isNew=false"></div>
	                </div>
	                <div class="content">
                		<div class="block" style="overflow: hidden;">
						    <span>设备名称</span>
						    <el-select v-model="equipmentName" 
						    	clearable 
						    	placeholder="请选择"
						    	@change='DeviceNameChange'>
						        <el-option
						          v-for="item in options"
						          :key="item.value"
						          :label="item.DeviceName"
						          :value="item.DeviceName">
						    	</el-option>
						    </el-select>
						</div>
					  	<div class="block">
						    <span>设备参数</span>
						  	<div class="autoGet">{{defualtData.DeviceParam}}</div>
						</div>
						<div class="block">
						    <span>设备型号</span>
						   <div class="autoGet">{{defualtData.DeviceVersion}}</div>
						</div>
						<div class="block">
						    <span>巡查周期</span>
						    <div class="autoGet">{{defualtData.CheckCycle}}</div>
						</div>
					  	<div class="block">
					  		<span>用途描述</span>
						  	<div class="discribe">{{defualtData.Description}}</div>
						</div>
						<div class="block">
						    <span>负责人</span>
						    <el-input v-model="equipmentPerson" placeholder="请输入内容"></el-input>
						</div>
						<div class="block time">
						    <span>最近运维时间</span>
						    <el-date-picker
						      v-model="equipmentTime"
						      type="date"
						      placeholder="选择日期"
						      @change='getAddTime'
						      format="yyyy-MM-dd"
						      value-format="yyyy-MM-dd">
						    </el-date-picker>
						</div>
						    <div class="block">
						    <span>备品备件更换情况</span>
						    <el-input v-model="equipmentChenge" placeholder="请输入内容"></el-input>
						</div>
						<el-row style='position: absolute;bottom: 20px;right: 30px;'>
							<el-button type="primary" @click='publish'>确定</el-button>
							<el-button plain @click='isNew=false'>取消</el-button>
						</el-row>
	               </div>
	            </div>
	        </div>
	        <!--------------编辑弹框部分--------------->
			<div class="popUp" v-if="isEdit">
	            <div class="mask"></div>
	            <div class="succ-pop">
	                <div class="title">
	                    <a id="newCreate">编辑</a>
	                    <div class="el-icon-close" @click="isEdit=false"></div>
	                </div>
	                <div class="content">
                		<div class="block" style="overflow: hidden;">
						    <span>设备名称</span>
						    <el-select v-model="equipmentEditName" 
						    	clearable 
						    	placeholder="请选择"
						    	@change='DeviceNameChange'>
						        <el-option
						          v-for="item in options"
						          :key="item.value"
						          :label="item.DeviceName"
						          :value="item.DeviceName">
						    	</el-option>
						    </el-select>
						</div>
					  	<div class="block">
						    <span>设备参数</span>
						  	<div class="autoGet">{{defualtData.DeviceParam}}</div>
						</div>
						<div class="block">
						    <span>设备型号</span>
						   <div class="autoGet">{{defualtData.DeviceVersion}}</div>
						</div>
						<div class="block">
						    <span>巡查周期</span>
						    <div class="autoGet">{{defualtData.CheckCycle}}</div>
						</div>
					  	<div class="block">
					  		<span>用途描述</span>
						  	<div class="discribe">{{defualtData.Description}}</div>
						</div>
						<div class="block">
						    <span>负责人</span>
						    <el-input v-model="equipmentEditPerson" placeholder="请输入内容"></el-input>
						</div>
						<div class="block time">
						    <span>最近运维时间</span>
						    <el-date-picker
						      v-model="equipmentEditTime"
						      type="date"
						      placeholder="选择日期"
						      @change='getEditTime'
						      format="yyyy-MM-dd"
						      value-format="yyyy-MM-dd">
						    </el-date-picker>
						</div>
						    <div class="block">
						    <span>备品备件更换情况</span>
						    <el-input v-model="equipmentEditChenge" placeholder="请输入内容"></el-input>
						</div>
						<el-row style='position: absolute;bottom: 20px;right: 30px;'>
							<el-button type="primary" @click='EditUpdate'>确定</el-button>
							<el-button plain @click='isEdit=false'>取消</el-button>
						</el-row>
	               </div>
	            </div>
	        </div>
		</div>
    </div>
</template>

<script>
    import {Message} from 'element-ui';
    import api from '../../../api/index'
    export default {
        name: 'businessOperation',
        data() {
            return {
            	//预警状态
               options: [],
		        value1: '',
		        value2: '',
		        value3: '',
		        value4: '',
		        value5: '',
		        value6: '',
		        tableData:[],
			    currentPage: 1,
			    pagesize:10,
			    isNew: false,
			    textarea: '',
			    title:'添加',
				//新建预警信息
				startTime:'',
				endTime:'',
				TotalRowsCount:null,
				totalCount:'',
				InfoData:[],
				ListData:[],
				Id:'',
				isend:false,
				//添加
				equipmentName:'',
				equipmentPerson:'',
				equipmentTime:'',
				equipmentChenge:'',
				//编辑
				equipmentEditName:'',
				equipmentEditPerson:'',
				equipmentEditTime:'',
				equipmentEditChenge:'',
				defualtData:{},
				isEdit:false,
				//级别
				optionsLevelVal:[{label:1,value:1},{label:2,value:2},{label:3,value:3}],
				levelVal:'',
				//组织
				optionsOrganizeVal:[{label:'环保局',value:'环保局'},
									{label:'大气办',value:'大气办'},
									{label:'建设局',value:'建设局'},
									{label:'综合执法队',value:'综合执法队'},
									{label:'固安镇',value:'固安镇'},
									{label:'牛驼镇',value:'牛驼镇'},
									{label:'马庄镇',value:'马庄镇'}
									],
				organizeVal:'',
				//岗位
				optionsStationVal:[{label:'二级网格员',value:'环保局'},
									{label:'二级网格长',value:'大气办'},
									{label:'分包领导',value:'建设局'},
									{label:'二级网格长个人账户',value:'综合执法队'}
									],
				stationVal:''
            }
        },
        created(){
        	this.getNotice();
        },
        mounted() {
        	this.GetOperDeviceInfo()
        },
        computed: {
            
        },
        methods: {
        	getAddTime(val){
        		this.equipmentTime = val;
        	},
        	getEditTime(val){
        		this.equipmentEditTime = val;
        	},
        	//列表删除
        	DeleteOperatorInfo(row) {
        		let t = this;
        		console.log(row)
        		let id = row.Id;
        		api.DeleteOperatorInfo(id).then(res=>{
      				console.log(res)
      			})
        		this.getNotice();
		    },
        	///新建预警信息发布
        	publish(){
        		this.Insert();
        		this.closeWin();
        		this.getNotice();
        	},
        	closeWin(){
      			this.isNew = false;
      		},
			//编辑
	        handleClick(row) {
	        	this.isEdit = true;
	        	console.log(row)
	        	if(this.isEdit){
	        		this.Id = row.Id;
	        		this.equipmentEditName = row.DeviceName;
	      			this.defualtData.DeviceParam = row.DeviceParam;
	      			this.defualtData.DeviceVersion = row.DeviceVersion;
	      			this.defualtData.CheckCycle = row.CheckCycle;
	      			this.defualtData.Description = row.Description;
	      			this.equipmentEditPerson = row.ChargeMan;
					this.equipmentEditTime = row.CreateTime;
					this.equipmentEditChenge = row.DeviceChangeInfo;
	        	}
	        	this.isNew = false;
      		},
      		//编辑发布
      		EditUpdate(){
      			let t = this;
      			let id = this.Id;
      			let DeviceName = '';
      			let DeviceParam = '';
      			let DeviceVersion = '';
      			let CheckCycle = '';
      			let Description = '';
      			let DeviceId = t.defualtData.Id;
      			let ChargeMan = t.equipmentEditPerson;
      			let CreateTime = t.equipmentEditTime;
      			let DeviceChangeInfo = t.equipmentEditChange		
      			api.UpdateOperatorInfo(id,DeviceId,DeviceName,DeviceParam,DeviceVersion,CheckCycle,Description,ChargeMan,CreateTime,DeviceChangeInfo).then(result=>{
					t.getNotice();
				});
				this.isEdit = false;
      		},
      		//分页
      		 handleSizeChange(val) {
        		console.log(`每页 ${val} 条`);
      		},
      		handleCurrentChange(val) {
        		this.setPageTable(10, val);
      		},
      		openWin(){
      			this.isEdit = false;
      			this.isNew = true;
      			this.equipmentName = '';
      			this.defualtData = '';
      			this.equipmentPerson = '';
				this.equipmentTime = '';
				this.equipmentChenge = '';
      		},
      		//添加设备名称选择
      		DeviceNameChange(val){
      			this.defualtData = this.options.find((item)=>{
      				return item.DeviceName === val;
      			})
      		},
      		//添加运维记录确定
      		Insert(){
      			let t = this;
      			let id = '';
      			let DeviceName = '';
      			let DeviceParam = '';
      			let DeviceVersion = '';
      			let CheckCycle = '';
      			let Description = '';
      			let DeviceId = t.defualtData.Id;
      			let ChargeMan = t.equipmentPerson;
      			let CreateTime = t.equipmentTime;
      			let DeviceChangeInfo = t.equipmentChenge;
				api.AddOperatorInfo(id,DeviceId,DeviceName,DeviceParam,DeviceVersion,CheckCycle,Description,ChargeMan,CreateTime,DeviceChangeInfo).then(result=>{
					
				});
      		},
      		//获取运维记录列表
      		getNotice(){
      			let t = this;
      			this.ListData = [];
      			api.GetOperatorInfo().then(result=>{
					let InfoData = result.data.Data;
					t.totalCount = InfoData.length;
					InfoData.forEach(item=>{
						let tableData = {};
                        tableData.DeviceName = item.DeviceName;//设备名称
                        tableData.CreateTime = item.CreateTime.replace('T',' ');//运维时间
                        tableData.CheckCycle = item.CheckCycle;//巡查周期
                        tableData.Description = item.Description;//用途描述
                        tableData.DeviceId = item.DeviceId;//设备id
                        tableData.Id = item.Id;//设备id
                        tableData.DeviceChangeInfo = item.DeviceChangeInfo;//设备更换情况
                        tableData.DeviceParam = item.DeviceParam;//设备参数
                        tableData.DeviceVersion = item.DeviceVersion;//设备型号
                        tableData.ChargeMan = item.ChargeMan;//负责人
                        t.ListData.push(tableData);
					})
					this.setPageTable(10, 1);
				});
      		},
      		//运维设备列表
      		GetOperDeviceInfo(){
      			let t = this;
      			api.GetOperDeviceInfo().then(res=>{
      				console.log(res);
      				t.options = res.data.Data;
      			})
      		},
      		 //分页数据
            setPageTable(pageSize, pageNum) {
                let i = 1;
                let rtValue = [];
                let startNum = pageSize * (pageNum - 1);
                for (let i = 0; i < pageSize; i++) {
                    if ((startNum + i + 1) > this.ListData.length)
                        break;
                    rtValue.push(this.ListData[startNum + i]);
                }
                this.tableData = rtValue;
            },
        },
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.businessOperation{
	.el-input{
		width: 215px;
	}
	#right{
		width: calc(100% - 200px);
		overflow: hidden;
		padding: 20px;
		background-color: #f6fbff;
		.box {
	        width: 100%;
	        height: auto;
	        .warning {
	        	text-align: left;
	            border-bottom: solid 1px #ccc;
	            width: 100%;
	            height: 40px;
	            margin-top: 10px;
	            margin-bottom: 20px;
	            margin-left: 10px;
	            a {
	                display: inline-block;
	                height: 20px;
	                border-left: solid 3px #428bca;
	                padding-left: 13px;
	                font-size: 16px;
	                line-height: 20px;
	            }
	        }
	    }
	    .search{
	    	text-align: left;
	    	margin-bottom: 24px;
	    	.searchBox{
	    		display: inline-block; 
	    		margin-right: 20px;
	    	}
	    	.block{
	    		display: inline-block;
	    	}
	    	.btns{
	    		margin-left: 40px;
	    	}
	    }
	    .page{
	    	text-align: left;
	    }
	    .eidt{
			color: #000;
			:hover{
		    	color: #20a0ff;
		    	text-decoration: underline;
	    	}
	    }
	    .InfoEnd{
	    	color: #000;
	    	:hover{
	    		color: #BF3831;
		    	text-decoration: underline;
	    	}
	    }
	    
	    .el-pagination{
	    	display: inline-block;
	    	margin-left: 170px;
	    	padding-bottom: 90px;
	    }
	    /*************弹出框**********/
	    .popUp {
	        /*灰色遮罩层*/
	        .mask {
	            width: 100%;
	            height: 100%;
	            background: rgba(0, 0, 0, 0.8);
	            position: fixed;
	            left: 0;
	            top: 0;
	            z-index: 998;
	        }
	        /*****弹出框内容********/
	        .succ-pop {
	            width: 515px;
	            height: 640px;
	            background: #fff;
	            position: fixed;
	            left: 50%;
	            top: 50%;
	            margin-left: -257px;
	            margin-top: -320px;
	            z-index: 999;
	            border-radius: 10px;
	            .title {
	                width: 100%;
	                height: 50px;
	                line-height: 50px;
	                text-align: left;
	                border-bottom: 2px solid #3a90b3;
	                a {
	                    color: #3a90b3;
	                    font-size: 18px;
	                    padding-left: 20px;
	                }
	                div {
	                    margin-top: 15px;
	                    float: right;
	                    width: 24px;
	                    height: 24px;
	                    color: #363636;
	                    margin-right: 6px;
	                }
	
	            }
	            .content{
	            	width: 400px;
	            	margin: 0 auto;
	            	background: #fff;
	            	span{
	            		display: inline-block;
	            		width: 120px;
	            		height: 40px;
	            		line-height: 40px;
	            		text-align: right;
	            		float: left;
	            	}
					.block{
						margin-top: 20px;
						span{
							margin-right: 10px;
						}
						.autoGet{
							/*display: inline-block;*/
							/*float: left;*/
							width: 215px;
							height: 40px;
							border: 1px solid #d1dbe4;
							border-radius: 4px;
							line-height: 40px;
							text-align: left;
							padding-left: 10px;
							background: #eef1f6;
							color: #7e807f;
							overflow: hidden;
							text-overflow:ellipsis;
							white-space: nowrap;
						}
						.discribe{
							height: 80px;
							padding: 15px 0;
							width: 215px;
							border: 1px solid #d1dbe4;
							border-radius: 4px;
							text-align: left;
							padding-left: 10px;
							background: #eef1f6;
							color: #7e807f;
							line-height: 18px;
							display: -webkit-box;
							-webkit-box-orient: vertical;
							-webkit-line-clamp: 3;
							overflow: hidden;
						}
					}
	            }
	            
	        }
	    }    
	}
}
</style>
