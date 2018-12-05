<template>
    <section>
        <!--工具条-->
        <el-col :span="24" class="toolbar" style="padding-bottom: 0px;">
            <el-form :inline="true" :model="filters">
                <el-form-item>
                    <el-input v-model="filters.id" placeholder="订单号"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" v-on:click="getUsers">查询</el-button>
                </el-form-item>
                <!--<el-form-item>
                    <el-button type="primary" @click="handleAdd">新增</el-button>
                </el-form-item>-->
            </el-form>
        </el-col>

        <!--列表-->
        <el-table :data="users" highlight-current-row v-loading="listLoading" @selection-change="selsChange"
                  style="width: 100%;">
            <el-table-column type="selection" width="55" disabled>
            </el-table-column>
            <el-table-column prop="id" label="订单号" width="180" sortable>
            </el-table-column>
            <el-table-column prop="source" label="来源" width="120" sortable>
            </el-table-column>

            <el-table-column prop="adultNumber" label="成人数量" width="120" sortable>
            </el-table-column>
            <el-table-column prop="childrenNumer" label="儿童数量" width="120" sortable>
            </el-table-column>
            <el-table-column prop="price" label="金额" width="100" sortable>
            </el-table-column>
            <el-table-column prop="payWay" label="支付方式" width="120" sortable>
            </el-table-column>
            <el-table-column prop="reserveTime" label="预定时间" width="170" sortable>
            </el-table-column>
            <el-table-column prop="operator" label="下单人" width="120" sortable>
            </el-table-column>
            <el-table-column prop="operationTime" label="下单时间" width="170" sortable>
            </el-table-column>
            <el-table-column prop="signInDay" label="签到时间" width="170" sortable>
            </el-table-column>
            <el-table-column prop="signOutDay" label="离开时间" width="170" sortable>
            </el-table-column>
            <el-table-column prop="status" label="状态" min-width="120" sortable>
                <template slot-scope="scope">
                    <span>{{ scope.row.status==0?'已预约':scope.row.status==1?'已接单':scope.row.status==2?'使用中':scope.row.status==3?'已完结':scope.row.status==4?'已取消':'已锁定' }}</span>
                </template>
            </el-table-column>
            <el-table-column label="操作" min-width="260">
                <template slot-scope="scope">
                    <el-button size="small" @click="handleEdit(scope.row.id, scope.row.status)"
                               :disabled="scope.row.status!=0">接单
                    </el-button>
                    <el-button type="primary" size="small" @click="checkIn(scope.row.id, scope.row.status)">
                        checkIn
                    </el-button>
                    <el-button type="primary" size="small" @click="checkOut(scope.row.id, scope.row.status)">
                        checkOut
                    </el-button>
                </template>
            </el-table-column>
        </el-table>

				<el-dialog title="选择温泉" :visible.sync="dialogRoomVisible">
					<ul class="product-list" ref="productList" v-if="sex=='0'">
						<li v-for="item in roomList" :class="'status'+item.status" @click="selectRoom($event,item.roomNo,item.status)">{{ item.roomNo }} <i :class="'icon iconstatus'+item.status"></i></li>
					</ul>
					<ul class="product-list1" ref="productList1" v-if="sex=='1'">
						<li v-for="item in roomList" :class="'status'+item.status" @click="selectRoom1($event,item.roomNo,item.status)">{{ item.roomNo }} <i :class="'icon iconstatus'+item.status"></i></li>
					</ul>
					<div class="radio-group" style="margin-left: 50%;transform: translateX(-21px);margin-top: 50px;">
						<el-radio-group v-model="sex" size="mini">
				      <el-radio-button :label="0">男宾</el-radio-button>
				      <el-radio-button :label="1">女宾</el-radio-button>
				    </el-radio-group>
					</div>
				  <div slot="footer" class="dialog-footer">
				    <el-button @click="dialogRoomVisible = false">取 消</el-button>
				    <el-button type="primary" @click="beforeCloseCheckIn">确 定</el-button>
				  </div>
				</el-dialog>
				
				
				<el-dialog title="CheckOut" :visible.sync="dialogCheckOutVisible" align="center">
				  <el-table :data="gridData">
				    <el-table-column property="invoice" label="是否打印发票" width="150"></el-table-column>
				    <el-table-column property="invoiceType" label="发票类型" width="100"></el-table-column>
				    <el-table-column property="Heading" label="抬头"></el-table-column>
				    <el-table-column property="dutyNum" label="税号"></el-table-column>
				    <el-table-column property="car" label="是否预约车"></el-table-column>
				    <el-table-column property="airStatus" label="是否飞机出行"></el-table-column>
				    <el-table-column property="airNum" label="航班号"></el-table-column>
				  </el-table>
				  <div slot="footer" class="dialog-footer">
				    <el-button @click="dialogCheckOutVisible = false">取 消</el-button>
				    <el-button type="primary" @click="beforeCloseCheckOut">确 定</el-button>
				  </div>
				</el-dialog>

        <!--工具条-->
        <el-col :span="24" class="toolbar">
            <el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量取消</el-button>
            <el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="20"
                           :total="total" style="float:right;">
            </el-pagination>
        </el-col>

        <!--编辑界面-->
        <el-dialog title="编辑" v-model="editFormVisible" :close-on-click-modal="false">
            <el-form :model="editForm" label-width="80px" :rules="editFormRules" ref="editForm">
                <el-form-item label="姓名" prop="name">
                    <el-input v-model="editForm.name" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="性别">
                    <el-radio-group v-model="editForm.sex">
                        <el-radio class="radio" :label="1">男</el-radio>
                        <el-radio class="radio" :label="0">女</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="年龄">
                    <el-input-number v-model="editForm.age" :min="0" :max="200"></el-input-number>
                </el-form-item>
                <el-form-item label="生日">
                    <el-date-picker type="date" placeholder="选择日期" v-model="editForm.birth"></el-date-picker>
                </el-form-item>
                <el-form-item label="地址">
                    <el-input type="textarea" v-model="editForm.addr"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click.native="editFormVisible = false">取消</el-button>
                <el-button type="primary" @click.native="editSubmit" :loading="editLoading">提交</el-button>
            </div>
        </el-dialog>

        <!--新增界面-->
        <el-dialog title="新增" v-model="addFormVisible" :close-on-click-modal="false">
            <el-form :model="addForm" label-width="80px" :rules="addFormRules" ref="addForm">
                <el-form-item label="姓名" prop="name">
                    <el-input v-model="addForm.name" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="性别">
                    <el-radio-group v-model="addForm.sex">
                        <el-radio class="radio" :label="1">男</el-radio>
                        <el-radio class="radio" :label="0">女</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="年龄">
                    <el-input-number v-model="addForm.age" :min="0" :max="200"></el-input-number>
                </el-form-item>
                <el-form-item label="生日">
                    <el-date-picker type="date" placeholder="选择日期" v-model="addForm.birth"></el-date-picker>
                </el-form-item>
                <el-form-item label="地址">
                    <el-input type="textarea" v-model="addForm.addr"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click.native="addFormVisible = false">取消</el-button>
                <el-button type="primary" @click.native="addSubmit" :loading="addLoading">提交</el-button>
            </div>
        </el-dialog>
    </section>
</template>

<script>
    import util from '../../common/js/util'
    //import NProgress from 'nprogress'
    import {getHotList, removeUser, batchRemoveUser, editUser, addUser, funDo, getOrderInfo} from '../../api/api';

    export default {
        data() {
            return {
            		dialogRoomVisible:false,
            		curRoomNo:"",
            		sex:"0",
            		roomList:[{
            			roomNo:"1001",
            			status:"0"
            		},{
            			roomNo:"1002",
            			status:"0"
            		},{
            			roomNo:"1003",
            			status:"0"
            		},{
            			roomNo:"1004",
            			status:"0"
            		},{
            			roomNo:"1005",
            			status:"1"
            		},{
            			roomNo:"1006",
            			status:"0"
            		},{
            			roomNo:"1007",
            			status:"0"
            		},{
            			roomNo:"1008",
            			status:"0"
            		},{
            			roomNo:"1009",
            			status:"0"
            		},{
            			roomNo:"1010",
            			status:"0"
            		},{
            			roomNo:"1011",
            			status:"0"
            		},{
            			roomNo:"1012",
            			status:"2"
            		},{
            			roomNo:"1013",
            			status:"0"
            		},{
            			roomNo:"1014",
            			status:"1"
            		},{
            			roomNo:"1015",
            			status:"0"
            		},{
            			roomNo:"1016",
            			status:"0"
            		}],
            		dialogCheckOutVisible:false,
            		gridData:[{
            			invoice:"是",
            			invoiceType:"个人",
            			Heading :"北京XX科技有限公司",
            			dutyNum :"XXXXXXXXXXXXX",
            			car:"是",
            			airStatus:"是",
            			airNum:"HB123s"
            		}],
            		operating:{
            			id:"",
            			status:""
            		},
                filters: {
                    id: ''
                },
                users: [],
                total: 1,
                page: 1,
                listLoading: false,
                sels: [],//列表选中列

                editFormVisible: false,//编辑界面是否显示
                editLoading: false,
                editFormRules: {
                    name: [
                        {required: true, message: '请输入姓名', trigger: 'blur'}
                    ]
                },
                //编辑界面数据
                editForm: {
                    id: 0,
                    name: '',
                    sex: -1,
                    age: 0,
                    birth: '',
                    addr: ''
                },

                addFormVisible: false,//新增界面是否显示
                addLoading: false,
                addFormRules: {
                    name: [
                        {required: true, message: '请输入姓名', trigger: 'blur'}
                    ]
                },
                //新增界面数据
                addForm: {
                    name: '',
                    sex: -1,
                    age: 0,
                    birth: '',
                    addr: ''
                }

            }
        },
        methods: {
            //性别显示转换
            formatSex: function (row, column) {
                return row.sex == 1 ? '男' : row.sex == 0 ? '女' : '未知';
            },
            handleCurrentChange(val) {
                this.page = val;
                this.getUsers();
            },
            //获取用户列表
            getUsers() {
                let para = {
                    id: this.filters.id
                };
                getHotList(para).then((res) => {
                    this.users = res.data.list;
                });
            },
            selectRoom(e,roomNo,status){
            	if(status!="0"){
            		return false;
            	}
            	let dom = document.querySelectorAll(".product-list li");
            	for(let i = 0;i<dom.length;i++){
            		if(dom[i].classList.contains("active")){
            			dom[i].classList.remove("active");
            		}
            	}
            	this.curRoomNo = roomNo;
            	e.target.classList.add("active");
            },
            selectRoom1(e,roomNo,status){
            	if(status!="0"){
            		return false;
            	}
            	let dom = document.querySelectorAll(".product-list1 li");
            	for(let i = 0;i<dom.length;i++){
            		if(dom[i].classList.contains("active")){
            			dom[i].classList.remove("active");
            		}
            	}
            	this.curRoomNo = roomNo;
            	e.target.classList.add("active");
            },
            checkIn: function (id, status) {
            		this.dialogRoomVisible = true;
								this.operating.id = id;
                this.operating.status = status;
            },
            beforeCloseCheckIn(){
            	if(this.curRoomNo==""){
            		this.$alert("请选择温泉编号", '提示', {
                    confirmButtonText: "确定",
                });
            		return false;
            	}else{
            		let _this = this;
            		let id = this.operating.id;
            		let status = this.operating.status;
                let param = {
                    funType: 2,
                    id: id,
                    type: '1',
                    rommMath:_this.curRoomNo
                };
                if (status == 1) {
                    funDo(param).then(function (res) {
                        if (res.data.statusCode == 200) {
                            _this.$alert(res.data.message, '提示', {
                                confirmButtonText: '确定',
                                callback: action => {
                                	_this.dialogRoomVisible = false;
                                  _this.getUsers();
                                }
                            });
                        }
                        else{
	                      	_this.$alert(res.data.message, '提示', {
	                              confirmButtonText: '确定',
	                              callback: action => {
	 
	                              }
	                          });
	                      }
                    })
                }
                else {
                    let dec = status == 0 ? '已预约' : status == 1 ? '已接单' : status == 2 ? '使用中' : status == 3 ? '已完结' : status == 4 ? '已取消' : '已锁定'
                    _this.$alert("订单状态为‘" + dec + "’不可checkIn", '提示', {
                        confirmButtonText: "确定",
                    });
                }
	            }
            },
            beforeCloseCheckOut(){
            	
	            	let _this = this;
	            	let id = this.operating.id;
          			let status = this.operating.status;
	              let param = {
	                  funType: 3,
	                  id: id,
	                  type: '1'
	              };
		            	if (status == 2) {
		                funDo(param).then(function (res) {
		                    if (res.data.statusCode == 200) {
		                        _this.$alert(res.data.message, '提示', {
		                            confirmButtonText: '确定',
		                            callback: action => {
		                            	_this.dialogCheckOutVisible = false;
		                                _this.getUsers();
		                            }
		                        });
		                    }
		                })
		            }
		            else {
		                let dec = status == 0 ? '已预约' : status == 1 ? '已接单' : status == 2 ? '使用中' : status == 3 ? '已完结' : status == 4 ? '已取消' : '已锁定'
		                _this.$alert("订单状态为‘" + dec + "’不可checkIn", '提示', {
		                    confirmButtonText: "确定",
		                });
		            }
            },
            checkOut(id, status){
            	let _this = this;
          		this.operating.id = id;
              this.operating.status = status;
              let param = {
              	preOrderNo:id
              }
              getOrderInfo(param).then(function (res) {
            		console.log(res);
                if (res.data.statusCode == 200) {
									if(res.data.list==""){
										_this.beforeCloseCheckOut();
										return false;
									}
									_this.gridData = res.data.list;
									_this.dialogCheckOutVisible = true;
                }
                else{
                	  _this.$alert(res.data.message, '提示', {
                        confirmButtonText: '确定',
                        callback: action => {
                             
                        }
                    });
                }
              })
            },
            //操作
            handleEdit: function (id, status) {
                let _this = this
                let param = {
                    funType: 1,
                    id: id,
                    type: '1'
                };
                funDo(param).then(function (res) {
                    if (res.data.statusCode == 200) {
                        _this.$alert(res.data.message, '提示', {
                            confirmButtonText: '确定',
                            callback: action => {
                                _this.getUsers();
                            }
                        });
                    }
                })
            },
            //显示新增界面
            handleAdd: function () {
                this.addFormVisible = true;
                this.addForm = {
                    name: '',
                    sex: -1,
                    age: 0,
                    birth: '',
                    addr: ''
                };
            },
            //编辑
            editSubmit: function () {
                this.$refs.editForm.validate((valid) => {
                    if (valid) {
                        this.$confirm('确认提交吗？', '提示', {}).then(() => {
                            this.editLoading = true;
                            //NProgress.start();
                            let para = Object.assign({}, this.editForm);
                            para.birth = (!para.birth || para.birth == '') ? '' : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd');
                            editUser(para).then((res) => {
                                this.editLoading = false;
                                //NProgress.done();
                                this.$message({
                                    message: '提交成功',
                                    type: 'success'
                                });
                                this.$refs['editForm'].resetFields();
                                this.editFormVisible = false;
                                this.getUsers();
                            });
                        });
                    }
                });
            },
            //新增
            addSubmit: function () {
                this.$refs.addForm.validate((valid) => {
                    if (valid) {
                        this.$confirm('确认提交吗？', '提示', {}).then(() => {
                            this.addLoading = true;
                            //NProgress.start();
                            let para = Object.assign({}, this.addForm);
                            para.birth = (!para.birth || para.birth == '') ? '' : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd');
                            addUser(para).then((res) => {
                                this.addLoading = false;
                                //NProgress.done();
                                this.$message({
                                    message: '提交成功',
                                    type: 'success'
                                });
                                this.$refs['addForm'].resetFields();
                                this.addFormVisible = false;
                                this.getUsers();
                            });
                        });
                    }
                });
            },
            selsChange: function (sels) {
                this.sels = sels;
            },
            //批量删除
            batchRemove: function () {
                let _this = this;
                let c = 0;
                let s = [];
                var ids = this.sels.map(item => item.id).toString();
                this.sels.forEach(function (sel) {
                    if (sel.status != 0) {
                        s.push(sel.status);
                        c++;
                    }
                });
                if (c > 0 && s[0] > 1) {
                    let text = s[0] == 2 ? '使用中' : s[0] == 3 ? '已完结' : s[0] == 4 ? '已取消' : '已锁定'
                    _this.$alert("订单状态为‘" + text + "’，无法取消，请重新选择", '提示', {
                        confirmButtonText: '确定',
                    });
                    return false
                }
                this.$confirm('确认取消选中订单吗？', '提示', {
                    type: 'warning'
                }).then(() => {
                    let para = {
                        id: ids,
                        funType: 4,
                        type: '1'
                    };
                    funDo(para).then((res) => {
                        if (res.data.statusCode == 200) {
                            this.$message({
                                message: '取消成功',
                                type: 'success'
                            });
                            this.getUsers();
                        }
                    });
                }).catch(() => {
                });
            }
        },
        mounted() {
            /*this.getUsers();*/
            getHotList().then((res) => {
                //this.total = res.data.total;
                this.users = res.data.list;
                //this.listLoading = false;
                //NProgress.done();
            });
        }
    }

</script>
<style $scoped lang="scss">
	.product-list{
		li{
			display: inline-block;
			width:119px;
			height:140px;
			line-height: 140px;
			border-radius: 10px;
			color:white;
			text-align: center;
			position: relative;
			margin-right:30px;
			margin-top:20px;
			cursor: pointer;
			.icon{
				width: 29px;
				height:37px;
				display: block;
				position: absolute;
				top:0;
				right:10px;
				background-size: contain;
				pointer-events: none;
			}
			.iconstatus0{
				background-image: url(../../assets/product/kong.png);
			}
			.iconstatus1{
				background-image: url(../../assets/product/weixiuzhuangtai.png);
			}
			.iconstatus2{
				background-image: url(../../assets/product/weixiuzhuangtai.png);
			}
		}
		li.active{
			box-shadow: 0 0 0.5rem rgba(47,85,130,.8);
			background-color: #4ac083!important;
			.icon{
				background-image: url(../../assets/product/yong.png)!important;
			}
		}
		li.status0{
			background-color: #a4c1eb;
		}
		li.status1{
			background-color: #bbbbbb;
		}
		li.status2{
			background-color: #bbbbbb;
		}
	}
	.product-list1{
		li{
			display: inline-block;
			width:119px;
			height:140px;
			line-height: 140px;
			border-radius: 10px;
			color:white;
			text-align: center;
			position: relative;
			margin-right:30px;
			margin-top:20px;
			cursor: pointer;
			.icon{
				width: 29px;
				height:37px;
				display: block;
				position: absolute;
				top:0;
				right:10px;
				background-size: contain;
				pointer-events: none;
			}
			.iconstatus0{
				background-image: url(../../assets/product/kong_w.png);
			}
			.iconstatus1{
				background-image: url(../../assets/product/yong.png);
			}
			.iconstatus2{
				background-image: url(../../assets/product/weixiuzhuangtai.png);
			}
		}
		li.active{
			box-shadow: 0 0 0.5rem rgba(47,85,130,.8);
			background-color: #4ac083!important;
			.icon{
				background-image: url(../../assets/product/yong.png)!important;
			}
		}
		li.status0{
			background-color: #ffbcbc;
		}
		li.status1{
			background-color: #4ac083;
		}
		li.status2{
			background-color: #bbbbbb;
		}
	}
</style>