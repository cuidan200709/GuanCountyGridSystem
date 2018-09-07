<!--后台管理-上报查询-->
<template>
    <div class="CaseReview">
		<!--上报查询右侧数据展示-->
		<div id="right">
			<!--上报查询-->
			<div class="box">
                <div class="warning">
                    <a>上报查询</a>
                </div>
            </div>
            <!--查询部分-->
			<div class="search">
				<div class="block" style="margin-top: 20px;">
				    <span class="demonstration">巡查员姓名</span>
				    <el-input v-model="patrollerName" placeholder="请输入内容" clearable></el-input>
				</div>
				<div class="block" style="margin-top: 20px;">
					 <span class="demonstration">起始时间</span>
					    <el-date-picker
					      v-model="CaseStartTime"
					      type="date"
					      value-format="yyyy-MM-dd"
					      placeholder="选择日期时间"
					      @change='startChange'>
					    </el-date-picker>
					    -
					    <el-date-picker
					      v-model="CaseEndTime"
					      type="date"
					      value-format="yyyy-MM-dd"
					      placeholder="选择日期时间"
					      @change='endChange'>
					    </el-date-picker>
					<el-button type="primary" class='btns' @click='GetMonitoringDay'>查询</el-button>
				    <el-button type="primary" class='btns' @click=''>导出</el-button>
				</div>
			</div>
			
			<!--列表部分-->
			<div class="box">
                <div class="warning">
                    <a>列表</a>
                </div>
           	</div>
           	<el-table
			    :data="ListData"
				border
			    style="width: 100%">
			    <el-table-column
			      prop="pollutiontype"
			      label="所属乡镇"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="status"
			      label="所属村庄"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="createtime"
			      label="巡查员姓名"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="location"
			      label="上报案件数量">
			    </el-table-column>
			    <el-table-column
			      prop="location"
			      label="误报率">
			    </el-table-column>
			</el-table>
		   	<div class="page">
			    <span class="demonstration">共找到{{totalCount}}条记录</span>
			    <el-pagination
				  background
			      @size-change="handleSizeChange"
			      @current-change="handleCurrentChange"
			      :current-page="currentPage"
			      :page-size="pagesize"
			      layout="prev, pager, next, jumper"
			      :total="totalCount">
			    </el-pagination>
			</div>
			<!--回复弹框部分-->

	        <!--查看弹框部分-->


		</div>
    </div>
</template>

<script>
    import {Message} from 'element-ui';
    import api from '../../../api/index'
    export default {
        name: 'CaseReview',
        data() {
            return {
                patrollerName:'',//
                CaseStartTime:'',//
                CaseEndTime:'',//
            	dialogImageUrl: '',
        		dialogVisible: false,
		        //责任主体
            	optionsDuty: [],
		        //污染类别
            	optionsPollution: [],
		        //分配弹框责任主体选择
		        optionsDistributePop: [],
            	//县市区选择
		        tableData:[],
			    currentPage: 1,
			    pagesize:10,
			    isConfirm: false,
			    Upload: false,
				//查询
				startTime:'',
				endTime:'',
				TotalRowsCount:null,
				totalCount:1,
				InfoData:[],
				ListData:[]
            }
        },
        created(){
        	
        },
        mounted() {

        },
        computed: {
            proStatus(){//上传状态
				if(this.pass){
					return 'success'
				}else if(this.pass == false){
					return 'exception'
				}else{
					return ''
				}
			}
        },
        methods: {

		   //处理结果
		   GetEditResult(){
		   		let t = this;
		   		let id = this.id;
		   		// let handlingResult = this.CaseDealPop;
		   		// let afterPath = this.fileUrl;
		   		// api.GetEditResult(id,handlingResult,afterPath).then(res=>{
		   		// 	console.log(res)
		   		// 	if(res.data.status){
		   		// 		t.GetMonitoringDay();
		   		// 		t.$message.success("填写处理结果成功")
		   		// 		t.isUpdate=false;
		   		// 	}
		   		//
		   		// })
		   },
        	//点击分配
        	handleDistrbuteClick(row){
        		console.log(row)
        		this.isDistribute = true;
        		this.id = row.id;
        	},
        	//点击回复
        	handleReplyClick(row){
        		this.isUpdate = true;
	         	this.PollutionClassPop = row.pollutiontype;//污染类别
		        this.CaseTimePop = row.createtime;//案发时间
		        this.CasePositionPop = row.location;//位置
		        this.CaseStatusPop = row.status;//案件状态
		        this.CaseDutyPop = row.departmenttype;//责任主体
		        this.textarea = row.description;//内容
		        this.tupian = row.tupian;
		        this.id = row.id;
        	},

            //开始时间选择
        	startChange(val){
        		this.startTime = val;
        	},
        	//结束时间选择
        	endChange(val){
        		this.endTime = val;
        	},
      		//分页
      		 handleSizeChange(val) {
        		console.log(`每页 ${val} 条`);
//      		this.GetMonitoringDay(10,val);
      		},
			//
      		handleCurrentChange(val) {
      			let t = this;
				let status;
      			this.ListData = [];
      			api.GetCaseList(status,departmenttype,pollutiontype,starTime,endTime,pageSize,pageNo).then(result=>{
      				console.log(result)
      				if(result){
      					let InfoData = result.data.list;
      					t.totalCount = result.data.count;
      					console.log(InfoData)
      					console.log('11')
      					if(InfoData){
      						InfoData.forEach(item=>{
                                // let tableData = {};
                                // tableData.casecode = item.casecode;
		                        // tableData.handlingResult = item.handlingResult;//处理结果
		                        t.ListData.push(tableData);
							})
      					}
						
//						this.setPageTable(10000, 1);
      				}
				});
      		},

      		//获取列表
      		GetMonitoringDay(){
      			let t = this;
				let status;

      			this.ListData = [];
      			api.GetCaseList(status,departmenttype,pollutiontype,starTime,endTime,pageSize,pageNo).then(result=>{
      				console.log(result)
      				if(result){
      					let InfoData = result.data.list;
      					t.totalCount = result.data.count;
      					console.log(InfoData)
      					console.log('11')
      					if(InfoData){
      						InfoData.forEach(item=>{
								let tableData = {};
                                // tableData.casecode = item.casecode;
		                        // tableData.createtime = item.createtime.replace('T',' ');//案发时间
		                        t.ListData.push(tableData);
							})
      					}
						
//						this.setPageTable(10000, 1);
      				}
				});
      		},
      		//导出
      		GetExportCase(){

      			//api.GetExportCase(status,departmenttype,pollutiontype,starTime,endTime,pageSize,pageNo);
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

		    //获取县市区数据（下拉框）
	        GetFirstGridDropDown(){
	        	//
	        }
        }, 
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

.img-list{
	overflow:hidden;
	width:100%;
}
.img-list .img-content{
	float:left;
	text-align:left;
	position:relative;
	display:inline-block;
	width:200px;
	height:200px;

	border:1px solid #d1dbe5;

}
.img-list .img-upload{
	float:left;
	width:200px;
	height:200px;
	display:table;
	text-align:center;
}
.img-list .uploader{
	width:100%;
	display:table-cell;
	vertical-align:middle;
}
.img-list .img-progress{
	text-align:center;
	padding-top:30px;
}
.img-list .img-content img{
	display:block;
	width:100%;
	height:200px;
	margin:0 auto;
	/*border-radius:4px;*/
}
.img-list .img-content .name{
	margin-top:10px;
}
.img-list .img-content .name>div{
	width:90%;
	text-overflow:ellipsis;
	overflow:hidden;
	height:25px;
	line-height:25px;
}
.img-list .img-content:hover .del,
.img-list .img-content:hover .layer{
	opacity:1;
}
.img-list .img-content .del,
.img-list .img-content .layer{
	opacity:0;
	transition:all .3s;
}
.img-list .img-content .del{
	position:absolute;
	bottom:10px;
	right:10px;
	color:#8492a6;
	cursor:pointer;
	font-size:1.1em;
}
.img-list .img-content .layer{
	position:absolute;
	left:0;
	right:0;
	top:0;
	height:200px;
	color:#fff;
	text-align:center;
	z-index:5;
	background-color:rgba(0,0,0,.4);
}
.img-list .img-content .layer i{
	font-size:1.6em;
	margin-top:80px;
}


.el-input, .el-input__inner{
	width: 200px;
}
.edit-input{
	width: 100px;
}
#right{
	width: 100%;
	overflow: hidden;
	padding: 20px;
	background-color: #f6fbff;
	.left{
		float: left;
	}
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
    	margin-left: 20px;
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
    	.InsertOrOut{
    		display: inline-block;
    		margin-left: 40px;
    		span{
    			a{
	    			color: #000000;
	    			font-size: 14px;
	    			margin-right: 40px;
    			}
    			:hover{
    				cursor: pointer;
    				color: #1797ff;
    				text-decoration: underline;
	    		}
    		}
    		
    	}
    }
    .page{
    	text-align: left;
    }  
    .el-pagination{
    	display: inline-block;
    	margin-left: 170px;
    	padding-bottom: 90px;
    }

}
</style>
