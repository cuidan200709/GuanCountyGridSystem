
<!--后台管理-监测点管理-->
<template>
    <div class="businessOperation">
		<!--监测点管理右侧数据展示-->
		<div id="right">
			<!--监测点管理-->
			<div class="box">
                <div class="warning">
                    <a>监测点管理</a>
                </div>
            </div>
            <!--查询部分-->
			<div class="search">
				<el-button type="primary" class='btns' @click="openWin">添加监测点</el-button>
			</div>
			
			<!--列表部分-->
			<div class="box">
                <div class="warning">
                    <a>列表</a>
                </div>
            </div>
            <el-table
			    :data="tableData"
				border
			    style="width: 100%">
			    <el-table-column
			      prop="DeviceName"
			      label="监测点名称"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="CreateTime"
			      label="监测点类别"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="CreateTime"
			      label="说明"
			      >
			    </el-table-column>
			    <el-table-column
			      label="操作"
			      width="180">
			      <template scope="scope">
			        <el-button @click="handleClick(scope.row)" type="text" size="small" class='eidt'>编辑</el-button>
			        <span style="color: #eee;">|</span>
			        <el-button @click="DeleteOperatorInfo(scope.row)" type="text" size="small" class='eidt'>删除</el-button>
			      </template>
			    </el-table-column>
			</el-table>
		   	<div class="page" style="">
			    <span class="demonstration">共找到{{totalCount}}条记录</span>
			    <el-pagination
			      @size-change="handleSizeChange"
			      @current-change="handleCurrentChange"
				  background
			      :current-page="currentPage"
			      :page-size="pagesize"
			      layout="prev, pager, next, jumper"
			      :total="totalCount">
			    </el-pagination>
			</div>
			<!--添加弹框部分-->
			<div class="popUp" v-show="isNew">
	            <div class="mask"></div>
	            <div class="succ-pop">
	                <div class="title">
	                    <a>添加</a>
	                    <div class="el-icon-close" @click="isNew=false"></div>
	                </div>
	                <div class="content">


	               </div>
	            </div>
	        </div>
	        <!--编辑弹框部分-->
			<div class="popUp" v-show="isEdit">
	            <div class="mask"></div>
	            <div class="succ-pop">
	                <div class="title">
	                    <a id="newCreate">编辑</a>
	                    <div class="el-icon-close" @click="isEdit=false"></div>
	                </div>
	                <div class="content">

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
				isEdit:false
            }
        },
        created(){

        },
        mounted() {
            this.getNotice();
        	//this.GetOperDeviceInfo()
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
	@import "../../../styles/houtaitanchuang";
.businessOperation{
	.el-input{
		width: 215px;
	}
	#right{
		width: 100%;
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

	}
}
</style>
