
<!--后台管理-企业污染源管理-->
<template>
    <div class="businessOperation">
		<!--企业污染源-->
		<div id="right">
			<!--运维记录管理-->
			<div class="box">
                <div class="warning">
                    <a>企业污染源</a>
                </div>
            </div>
            <!--查询部分-->
			<div class="search">
				<span>企业名称</span><el-input v-model="componyName" placeholder="请输入内容"></el-input>
				<!--<span>网格名称</span><el-input v-model="gridName" placeholder="请输入内容"></el-input>-->
				<el-button type="primary" class='btns' @click="QueryNeedsData">查询</el-button>
				<el-button type="primary" class='btns' @click="openWin">添加企业</el-button>
				<el-button type="primary" class='btns' @click="">导出</el-button>
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
			      prop="psname"
			      label="企业名称"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="pollutionAddress"
			      label="地址"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="contactPerson"
			      label="联系人"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="mobilePhone"
			      label="联系方式"
			      >
			    </el-table-column>

			    <el-table-column
			      prop="latitude"
			      label="经度"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="longitude"
			      label="纬度"
			      >
			    </el-table-column>

			    <el-table-column
			      label="操作"
			      width="160">
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
			<!---->
			<el-dialog title="企业污染源管理" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
				<!---->
				<el-tabs v-model="activeName" @tab-click="handleClick">
					<el-tab-pane label="排放口编辑" name="first">

					<!---->
					</el-tab-pane>
					<el-tab-pane label="企业信息编辑" name="second">
					<!---->

					</el-tab-pane>

				</el-tabs>
				<!---->
				<span slot="footer" class="dialog-footer">
					<el-button @click="dialogVisible = false">取 消</el-button>
					<el-button type="primary" @click="dialogVisible = false">确 定</el-button>
			  	</span>
			</el-dialog>
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
                dialogVisible:false,
		        tableData:[{DeviceName:'0000'}],
			    currentPage: 1,
			    pagesize:10,
                activeName:'first',
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
				componyName:'',
				gridName:''
            }
        },
        created(){

        },
        mounted() {

            this.getNotice();
        },
        computed: {
            
        },
        methods: {
            QueryNeedsData(){
                let condata = this.componyName;
                this.getNotice(condata);
            },
            //列表删除
            DeleteOperatorInfo(row) {
                const _this = this;
                console.log(row)
                let id = row.pscode;
                this.$confirm('确认要删除本条数据吗？')
                    .then(_ => {
                        // done();
                        console.log('删除成功')
                        api.GetdeleteCompanyRt(id).then(res=>{
                            console.log(res)
                            if(res.data.Status === 1){
                                _this.$message({showClose: true, message: '删除成功', type: 'success'});
                            }else {
                                _this.$message({showClose: true, message: '删除失败', type: 'error'});
                            }
                        })
                        //
                        setTimeout(()=>{
                            _this.getNotice();
                        },200)
                    })
                    .catch(_ => {
                        console.log('删除失败')
                    });
            },
        	///新建预警信息发布
        	publish(){
        		this.Insert();
        		this.closeWin();
        		this.getNotice();
        	},
        	closeWin(){
      			this.dialogVisible = false;
      		},
			//编辑
	        handleClick(row) {
	        	this.dialogVisible = true;
	        	console.log(row)
	        	if(this.dialogVisible){
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
	        	this.dialogVisible = false;
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
				this.dialogVisible = false;
      		},
      		//分页
      		 handleSizeChange(val) {
        		console.log(`每页 ${val} 条`);
      		},
      		handleCurrentChange(val) {
        		// this.setPageTable(10, val);
                this.getNotice('',val);
      		},
      		openWin(){
      			this.isEdit = false;
      			this.dialogVisible = true;
      			this.equipmentName = '';
      			this.defualtData = '';
      			this.equipmentPerson = '';
				this.equipmentTime = '';
				this.equipmentChenge = '';
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
      		getNotice(name = '',PageIndex = '1'){
        	    let nm =name;
        	    let page = PageIndex;
      			const _this = this;
      			this.tableData = [];
      			api.PosthtcompanyListRt(nm,page).then(result=>{
      			    console.log(result)
					let InfoData = result.data.Data.Data;

                    _this.totalCount = result.data.Data.TotlePageNum;
					InfoData.forEach(item=>{
						let tableData = {};
                        tableData.psname = item.psname;//设备名称
                        tableData.contactPerson = item.contactPerson;//设备名称
                        tableData.mobilePhone = item.mobilePhone;//设备名称
                        tableData.pollutionAddress = item.pollutionAddress;//设备名称
                        tableData.pscode = item.pscode;//设备名称
                        tableData.latitude = item.latitude;//设备名称
                        tableData.longitude = item.longitude;//设备名称
                        _this.tableData.push(tableData);
					})

				});
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
