
<!--后台管理-巡查员管理-->
<template>
    <div class="businessOperation">
		<!--右侧数据展示-->
		<div id="right">
			<div class="box">
                <div class="warning">
                    <a>巡查员管理</a>
                </div>
            </div>
			<div class="search">
				<span>巡查员名称</span><el-input v-model="departmentVal" placeholder="请输入内容"></el-input>
				<el-button type="primary" class='btns' @click="QueryNeedsData">查询</el-button>
				<el-button type="primary" class='btns' @click="openWin">添加巡查员</el-button>
				<el-button type="primary" class='btns' @click="Export">导出</el-button>
			</div>
			<!--list-->
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
			      prop="name"
			      label="姓名"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="username"
			      label="登录名"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="role"
			      label="岗位"
			     >
			    </el-table-column>
			    <el-table-column
			      prop="dlApp"
			      label="登录APP"
			      >
			    </el-table-column>
			    <el-table-column
			      prop="statusAj"
			      label="处理案件"
			      >
			    </el-table-column>

			    <el-table-column
			      label="操作"
			      width="180">
			      <template scope="scope">
			        <el-button @click="handleClick(scope.row)" type="text" size="small" class='eidt'>编辑</el-button>
			        <span style="color: #e0e0e0;">|</span>
			        <el-button @click="changePassword(scope.row)" type="text" size="small" class='eidt'>修改密码</el-button>
			        <span style="color: #e0e0e0;">|</span>
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
						<div class="block">
							<span>姓名</span>
							<el-input v-model="equipmentPerson1" placeholder="请输入内容"></el-input>
						</div>
						<div class="block">
							<span>登录名</span>
							<el-input v-model="equipmentPerson2" placeholder="请输入内容"></el-input>
						</div>
						<div class="block">
							<span>岗位</span>
							<el-input v-model="equipmentPerson3" placeholder="请输入内容"></el-input>
						</div>
						<div class="block">
							<span>经度</span>
							<el-input v-model="equipmentPerson4" placeholder="请输入内容"></el-input>
						</div>
						<div class="block">
							<span>纬度</span>
							<el-input v-model="equipmentPerson5" placeholder="请输入内容"></el-input>
						</div>
                		<div class="block" style="overflow: hidden;">
						    <span>登录权限</span>
						    <el-select v-model="equipmentName1"
						    	clearable 
						    	placeholder="请选择"
						    	@change='DeviceNameChange1'>
								<el-option key="0" label="禁止" value="0"></el-option>
								<el-option key="1" label="允许" value="1"></el-option>
						    </el-select>
						</div>
						<div class="block" style="overflow: hidden;">
							<span>处理权限</span>
							<el-select v-model="equipmentName2"
									   clearable
									   placeholder="请选择"
									   @change='DeviceNameChange2'>
								<el-option key="0" label="禁止" value="0"></el-option>
								<el-option key="1" label="允许" value="1"></el-option>
							</el-select>
						</div>
						<el-row style='position: absolute;bottom: 20px;right: 30px;'>
							<el-button type="primary" @click='publish'>确定</el-button>
							<el-button plain @click='isNew=false'>取消</el-button>
						</el-row>
	               </div>
	            </div>
	        </div>
	        <!--编辑弹框部分-->
			<div class="popUp" v-show="isEdit">
	            <div class="mask"></div>
	            <div class="succ-pop">
	                <div class="title">
	                    <a>编辑</a>
	                    <div class="el-icon-close" @click="isEdit=false"></div>
	                </div>
	                <div class="content">
						<div class="block">
							<span>姓名</span>
							<el-input v-model="equipmentPerson1b" placeholder="请输入内容"></el-input>
						</div>
						<div class="block">
							<span>登录名</span>
							<el-input v-model="equipmentPerson2b" placeholder="请输入内容"></el-input>
						</div>
						<div class="block">
							<span>岗位</span>
							<el-input v-model="equipmentPerson3b" placeholder="请输入内容"></el-input>
						</div>
						<div class="block">
							<span>经度</span>
							<el-input v-model="equipmentPerson4b" placeholder="请输入内容"></el-input>
						</div>
						<div class="block">
							<span>纬度</span>
							<el-input v-model="equipmentPerson5b" placeholder="请输入内容"></el-input>
						</div>
						<div class="block" style="overflow: hidden;">
							<span>登录权限</span>
							<el-select v-model="equipmentName1b"
									   clearable
									   placeholder="请选择"
									   @change='DeviceNameChange3'>
								<el-option key="0" label="禁止" value="0"></el-option>
								<el-option key="1" label="允许" value="1"></el-option>
							</el-select>
						</div>
						<div class="block" style="overflow: hidden;">
							<span>处理权限</span>
							<el-select v-model="equipmentName2b"
									   clearable
									   placeholder="请选择"
									   @change='DeviceNameChange4'>
								<el-option key="0" label="禁止" value="0"></el-option>
								<el-option key="1" label="允许" value="1"></el-option>
							</el-select>
						</div>
						<el-row style='position: absolute;bottom: 20px;right: 30px;'>
							<el-button type="primary" @click='EditUpdate'>确定</el-button>
							<el-button plain @click='isEdit=false'>取消</el-button>
						</el-row>
	               </div>
	            </div>
	        </div>
			<!--修改密码-->
			<el-dialog title="修改密码" :visible.sync="dialogVisible" width="30%">
				<div class="block">
					<span>新密码：</span>
					<el-input v-model="newpossword" placeholder="请输入新密码" @change="sysUserPasswordChange"></el-input>
					<div class="tishi" v-show="classts">{{tishiyu}}</div>
				</div>
				<span slot="footer" class="dialog-footer">
				<el-button @click="dialogVisible = false">取 消</el-button>
				<el-button type="primary" @click="PasswordChangeSubmit">确 定</el-button>
			  </span>
			</el-dialog>
		</div>
    </div>
</template>

<script>
    //import {Message} from 'element-ui';
    import api from '../../../api/index'
    export default {
        name: 'businessOperation',
        data() {
            return {
                //
                departmentVal:'',
				//
                dialogVisible:false,
				//
                newpossword:'',
				//
                tishiyu:'',
				//
                classts:false,
				//
		        tableData:[{DeviceName:'00000'}],
			    currentPage: 1,
                totalCount:1,
			    pagesize:10,
			    isNew: false,
			   	//
			    title:'添加',
				//
				TotalRowsCount:null,
				InfoData:[],
				ListData:[],
				isend:false,
				//添加
                equipmentPerson1:'',
                equipmentPerson2:'',
                equipmentPerson3:'',
                equipmentPerson4:'',
                equipmentPerson5:'',
                equipmentPersonid:'',
                equipmentName1:'',
                equipmentName2:'',
				//编辑
                equipmentPerson1b:'',
                equipmentPerson2b:'',
                equipmentPerson3b:'',
                equipmentPerson4b:'',
                equipmentPerson5b:'',
                equipmentPersonidb:'',
                equipmentName1b:'',
                equipmentName2b:'',
				isEdit:false,
				//
                lodid:'',
            }
        },
        created(){

        },
        mounted() {
        	//
            this.getNotice();
        },
        computed: {
            
        },
        methods: {
            //查询
            QueryNeedsData(){
                let condata = this.departmentVal;
                this.getNotice(condata);
			},
            //列表删除
            DeleteOperatorInfo(row) {
                const _this = this;
                console.log(row)
                let id = row.userId;
                this.$confirm('确认要删除本条数据吗？')
                    .then(_ => {
                        // done();
                        console.log('删除成功')
                        api.POSTsysUserdeletet(id).then(res=>{
                            console.log(res)
                            if(res.data.message === 'true'){
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
                //
        		this.Insert();
                this.isNew = false;
        	},
			//修改密码初始化
			changePassword(row){
                console.log(row.userId);
                this.lodid = row.userId
                this.dialogVisible = true;
			},
			//修改密码于旧密码比较
            sysUserPasswordChange(){
                let password = this.newpossword;
                api.GetsysUserPassword(password).then(res =>{
                    console.log(res);
                    //let data = res.data.Data.Data;

					if(res.data.message === 'true'){
                        this.tishiyu = '密碼可以使用';
                        this.offend = true;
                        this.classts = true;
					}else {
                        this.tishiyu = '密碼不可以使用,與舊密碼重複';
                        this.offend = false;
                        this.classts = true;
					}

				})
				if(!password){
                    this.classts =false;
				}
			},
			//修改密码提交
            PasswordChangeSubmit(){
				//
				let offend = this.offend;
                let userId = this.lodid;
                let password = this.newpossword;
                if(offend === true){
                    api.PostchangePassword(userId,password).then(res =>{
                        console.log(res);
                    })
				}else {
                    return false
				}

                this.dialogVisible = false;
			},
			//编辑
	        handleClick(row) {
                console.log(row);
	        	this.isEdit = true;
	        	this.equipmentPersonidb = row.userId;
	        	this.equipmentPerson1b = row.name;
	      		this.equipmentPerson2b = row.username;
	      		this.equipmentPerson3b = row.role;
	      		this.equipmentName1b = row.dlApp;
	      		this.equipmentName2b = row.statusAj ;
	        	this.isNew = false;
      		},
      		//编辑发布
      		EditUpdate(){
      			let _this = this;
      			let userId = _this.equipmentPersonidb;
      			let username = _this.equipmentPerson2b;
      			let name = _this.equipmentPerson1b;
                let status = _this.contenterChange(_this.equipmentName1b);
      			let role = _this.contenterChange(_this.equipmentName2b);
				//
      			api.POSTsysUserupdatet(userId,username,status,role,name).then(result=>{
      			    console.log(result);
                    _this.getNotice();
				});
				this.isEdit = false;
      		},
			//转换状态传值
			contenterChange(val){
                let keydata = val;
                let data = null;
                if(keydata === '允许'){
                    data = 1;
				}else if (keydata === '禁止'){
                    data = 0;
				} else {
                    data = keydata;
				}
                //isNaN(val) === true && return val;
                return data;
			},
      		//分页
      		 handleSizeChange(val) {
        		console.log(`每页 ${val} 条`);
      		},
			//分页请求
      		handleCurrentChange(val) {
        		this.setPageTable(10, val);
      		},
			//打开添加
      		openWin(){
      			this.isEdit = false;
      			this.isNew = true;
      			this.equipmentPersonid ='';
      			this.equipmentPerson1 = '';
      			this.equipmentPerson2 = '';
      			this.equipmentPerson3 = '';
				this.equipmentName1 = '';
				this.equipmentName2 = '';

      		},
      		//添加登录权限
      		DeviceNameChange1(val){
                console.log(val);
                this.equipmentName1 = val;
			},
            //添加处理权限
            DeviceNameChange2(val){
                console.log(val);
                this.equipmentName2 = val;
			},
            //编辑登录权限
            DeviceNameChange3(val){
                console.log(val);
                this.equipmentName1b = val;
			},
            //编辑处理权限
            DeviceNameChange4(val){
                console.log(val);
                this.equipmentName2b = val;
			},
      		//添加请求
      		Insert(){
      			const _this = this;
      			let userId = _this.equipmentPersonid ||'';
      			let username = _this.equipmentPerson2;
      			let status = _this.equipmentName1;
      			let role = _this.equipmentName2;
      			let name = _this.equipmentPerson1;
				api.POSTsysUseraddt(userId,username,status,role,name).then(result=>{
                    _this.getNotice();
				});
      		},
      		//获取运维记录列表
      		getNotice(name = ''){
                let nm =name;
      			const _this = this;
      			this.ListData = [];
      			api.GetsysUserlistdt(nm).then(result=>{
      			    console.log(result);
					let InfoData = result.data.data;
                    _this.totalCount = InfoData.length;
					InfoData.forEach(item=>{
						let tableData = {};
                        tableData.statusAj = item.role ? '允许' : '禁止';//
                        tableData.username = item.username;//
                        tableData.sex = item.sex;//
                        tableData.role = item.role ? '责任主体人员':'巡查员';//
                        tableData.password = item.password;//
                        tableData.name = item.name;//
                        tableData.mobile = item.mobile;//
                        //tableData.lastLoginTime = this.timestampToTime(item.lastLoginTime);//
						tableData.dlApp = item.status ?'允许' : '禁止';
                        //tableData.lastLoginIp = item.lastLoginIp;//
                        tableData.email = item.email;//
                        //tableData.createTime = _this.timestampToTime(item.createTime);//
                        tableData.userId = item.userId;//
                        _this.ListData.push(tableData);
					})
                    _this.setPageTable(10, 1);
				});
      		},
			//时间戳转换时间
            timestampToTime(timestamp) {
                let date = new Date(timestamp);//如果date为10位不需要乘1000
                let Y = date.getFullYear() + '-';
                let M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-';
                let D = (date.getDate() < 10 ? '0' + (date.getDate()) : date.getDate()) + ' ';
                let h = (date.getHours() < 10 ? '0' + date.getHours() : date.getHours()) + ':';
                let m = (date.getMinutes() <10 ? '0' + date.getMinutes() : date.getMinutes()) + ':';
                let s = (date.getSeconds() <10 ? '0' + date.getSeconds() : date.getSeconds());
                return Y+M+D+h+m+s;
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
			//导出
            Export(){
                api.ExportInspectorExcel();
			}
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
