<template>
	<div>
		<!-- 面包屑导航区 -->
		<el-breadcrumb separator="/">
			<el-breadcrumb-item>首页</el-breadcrumb-item>
			<el-breadcrumb-item>所有设备</el-breadcrumb-item>
		</el-breadcrumb>
		<!-- 卡片视图区 -->
		<el-card>
			<!--搜索区 -->
			<div class="nav" style="margin-top: 15px;">
				<el-row :gutter="20">
					<el-col :span="18">
						<el-input
							placeholder="请输入内容"
							v-model="queryInfo.query"
							class="input-with-select"
							clearable
							@clear="getDevicesList"
						>
							<el-button slot="append" icon="el-icon-search" @click="Search"></el-button>
						</el-input>
					</el-col>
					<el-button  id='showinfo' round plain type="info" @click="showAllinfo" style="margin-right:0px" size="mini">显示设备所有信息</el-button>
				</el-row>
			</div>
		<!-- 设备筛选区 1-->
			<el-collapse v-model="activenames" >
				<!-- 默认关闭，打开在el-collapse-item 加上 name = "1" -->
				<el-collapse-item title="设备筛选 : >>>" name="1" > 
					<div id ="choice">
						<section id="section">
							<!-- <nav id="choose">
								<mark v-for="(item,key) in choose" :key="item"  @click="delFn(key)">
									{{item}}
									<a href="javascript:;">x</a>
								</mark>
							</nav> -->
							<ul id="type" v-if="dataList !==null">
								<li>
									{{dataList[0].title}}:
									<template >
									<!-- <el-checkbox :id=i indeterminate="isIndeterminate" v-model="checkAll" @change="channge1(i)" :checked='test11' a='tttt'>全选</el-checkbox> -->
									<!-- <el-checkbox id="i" :v-model="checkAll0" @change="allchannge0" >全选</el-checkbox> -->
										<el-checkbox-group v-model="queryInfo.filter_list.brand" @change="singleChange0">
											<el-checkbox  v-for="(value,index) in dataList[0].list" :label="value" :key="value"  v-show="isOpen|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen && dataList[0].list.length>Max" @click="isOpen =!isOpen" type="text">展开 </el-button>
										<el-button v-show="isOpen && dataList[0].list.length>Max" @click="isOpen =!isOpen"  type="text">收起 </el-button>
									</template>							
									<!-- <a v-for="(value,j) in item.list"  :key="j" href="javascript:;" v-bind:class="{active:item.index===j}" @click="showFn(value,i,j)">
									{{value}}    
									</a> -->
								</li>
							<li >
									{{dataList[1].title}}:  
									<template>
									<!-- <el-checkbox :id=i indeterminate="isIndeterminate" v-model="checkAll" @change="channge1(i)" :checked='test11' a='tttt'>全选</el-checkbox> -->
									<!-- <el-checkbox id="i" :v-model="checkAll1" @change="allchannge1" >全选</el-checkbox> -->
										<el-checkbox-group v-model="android" @change="singleChange1">
											<el-checkbox :disabled="show2" v-for="(value,index) in dataList[1].list" :label="value" :key="value"  v-show="isOpen1|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen1 && dataList[1].list.length>Max" @click="isOpen1 =!isOpen1" type="text">展开 </el-button>
										<el-button v-show="isOpen1 && dataList[1].list.length>Max" @click="isOpen1 =!isOpen1"  type="text">收起 </el-button>
									</template>							
								</li>
							<li >
									{{dataList[2].title}}:  
									<template>
									<!-- <el-checkbox :id=i indeterminate="isIndeterminate" v-model="checkAll" @change="channge1(i)" :checked='test11' a='tttt'>全选</el-checkbox> -->
									<!-- <el-checkbox id="i" :v-model="checkAll2" @change="allchannge2" >全选</el-checkbox> -->
										<el-checkbox-group v-model="ios" @change="singleChange2">
											<el-checkbox :disabled="show3" v-for="(value,index) in dataList[2].list" :label="value" :key="value"  v-show="isOpen2|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen2 && dataList[2].list.length>Max" @click="isOpen2 =!isOpen2" type="text">展开 </el-button>
										<el-button v-show="isOpen2 && dataList[2].list.length>Max" @click="isOpen2 =!isOpen2"  type="text">收起 </el-button>
									</template>							
								</li>
							<li >
								<!-- CPU -->
									{{dataList[3].title}}: 	
									<template class="filter">
										<span>高通: </span>
										<el-checkbox-group v-model="queryInfo.filter_list.cpu" @change="singleChange3">
											<el-checkbox  v-for="(value,index) in dataList[3].dict.gt" :label="value" :key="value" v-show="isOpen3|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen3 && dataList[3].dict.gt.length>Max" @click="isOpen3 =!isOpen3" type="text">展开 </el-button>
										<el-button v-show="isOpen3 && dataList[3].dict.gt.length>Max" @click="isOpen3 =!isOpen3"  type="text">收起 </el-button>									
									</template>	
									<div></div>
									<template>
										<span style="margin-left:35px">麒麟: </span>
										<el-checkbox-group v-model="queryInfo.filter_list.cpu" @change="singleChange3">
											<el-checkbox  v-for="(value,index) in dataList[3].dict.hw" :label="value" :key="value" v-show="isOpen31|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen31 && dataList[3].dict.hw.length>Max" @click="isOpen31 =!isOpen31" type="text">展开 </el-button>
										<el-button v-show="isOpen31 && dataList[3].dict.hw.length>Max" @click="isOpen31 =!isOpen31"  type="text">收起 </el-button>									
									</template>	
									<div></div>
									<template>
										<span style="margin-left:35px">苹果: </span>
										<el-checkbox-group v-model="queryInfo.filter_list.cpu" @change="singleChange3">
											<el-checkbox  v-for="(value,index) in dataList[3].dict.pg" :label="value" :key="value" v-show="isOpen32|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen32 && dataList[3].dict.pg.length>Max" @click="isOpen32 =!isOpen32" type="text">展开 </el-button>
										<el-button v-show="isOpen32 && dataList[3].dict.pg.length>Max" @click="isOpen32 =!isOpen32"  type="text">收起 </el-button>									
									</template>	
									<div></div>
									<template>
										<span style="margin-left:35px">其他: </span>
										<el-checkbox-group v-model="queryInfo.filter_list.cpu" @change="singleChange3">
											<el-checkbox  v-for="(value,index) in dataList[3].dict.qt" :label="value" :key="value" v-show="isOpen33|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen33 && dataList[3].dict.qt.length>Max" @click="isOpen33 =!isOpen33" type="text">展开 </el-button>
										<el-button v-show="isOpen33 && dataList[3].dict.qt.length>Max" @click="isOpen33 =!isOpen33"  type="text">收起 </el-button>									
									</template>																												

								</li>
								<!-- GPU -->
							<li >
									{{dataList[4].title}}:  
									<template>
										<span>Adreno: </span>
										<el-checkbox-group v-model="queryInfo.filter_list.gpu" @change="singleChange4">
											<el-checkbox v-for="(value,index) in dataList[4].dict.gt" :label="value" :key="value"  v-show="isOpen4|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen4 && dataList[4].dict.gt.length>Max" @click="isOpen4 =!isOpen4" type="text">展开 </el-button>
										<el-button v-show="isOpen4 && dataList[4].dict.gt.length>Max" @click="isOpen4 =!isOpen4"  type="text">收起 </el-button>
									</template>		
									<div></div>
									<template>
										<span style="margin-left:35px">Mali: </span>
										<el-checkbox-group v-model="queryInfo.filter_list.gpu" @change="singleChange4">
											<el-checkbox v-for="(value,index) in dataList[4].dict.arm" :label="value" :key="value"  v-show="isOpen41|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen41 && dataList[4].dict.arm.length>Max" @click="isOpen41 =!isOpen41" type="text">展开 </el-button>
										<el-button v-show="isOpen41 && dataList[4].dict.arm.length>Max" @click="isOpen41 =!isOpen41"  type="text">收起 </el-button>
									</template>	
									<div></div>
									<template>
										<span style="margin-left:35px">ios: </span>
										<el-checkbox-group v-model="queryInfo.filter_list.gpu" @change="singleChange4">
											<el-checkbox v-for="(value,index) in dataList[4].dict.pg" :label="value" :key="value"  v-show="isOpen42|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen42 && dataList[4].dict.pg.length>Max" @click="isOpen42 =!isOpen42" type="text">展开 </el-button>
										<el-button v-show="isOpen42 && dataList[4].dict.pg.length>Max" @click="isOpen42 =!isOpen42"  type="text">收起 </el-button>
									</template>	
									<div></div>
									<template>
										<span style="margin-left:35px">PowerVR: </span>
										<el-checkbox-group v-model="queryInfo.filter_list.gpu" @change="singleChange4">
											<el-checkbox v-for="(value,index) in dataList[4].dict.img" :label="value" :key="value"  v-show="isOpen43|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen43 && dataList[4].dict.img.length>Max" @click="isOpen43 =!isOpen43" type="text">展开 </el-button>
										<el-button v-show="isOpen43 && dataList[4].dict.img.length>Max" @click="isOpen43 =!isOpen43"  type="text">收起 </el-button>
									</template>																																	
								</li>
							<li >
									{{dataList[5].title}}:  
									<template>
									<!-- <el-checkbox :id=i indeterminate="isIndeterminate" v-model="checkAll" @change="channge1(i)" :checked='test11' a='tttt'>全选</el-checkbox> -->
									<!-- <el-checkbox id="i" :v-model="checkAll5" @change="allchannge5" >全选</el-checkbox> -->
										<el-checkbox-group v-model="queryInfo.filter_list.ram" @change="singleChange5">
											<el-checkbox v-for="(value,index) in dataList[5].list" :label="value" :key="value"  v-show="isOpen5|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen5 && dataList[5].list.length>Max" @click="isOpen5 =!isOpen5" type="text">展开 </el-button>
										<el-button v-show="isOpen5 && dataList[5].list.length>Max" @click="isOpen5 =!isOpen5"  type="text">收起 </el-button>
									</template>							
								</li>
							<li >
									{{dataList[6].title}}:  
									<template>
									<!-- <el-checkbox :id=i indeterminate="isIndeterminate" v-model="checkAll" @change="channge1(i)" :checked='test11' a='tttt'>全选</el-checkbox> -->
									<!-- <el-checkbox id="i" :v-model="checkAll6" @change="allchannge6" >全选</el-checkbox> -->
										<el-checkbox-group v-model="queryInfo.filter_list.screen" @change="singleChange6">
											<el-checkbox v-for="(value,index) in dataList[6].list" :label="value" :key="value"  v-show="isOpen6|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen6 && dataList[6].list.length>Max" @click="isOpen6 =!isOpen6" type="text">展开 </el-button>
										<el-button v-show="isOpen6 && dataList[6].list.length>Max" @click="isOpen6 =!isOpen6"  type="text">收起 </el-button>
									</template>							
								</li>
							<li >
									{{dataList[7].title}}:  
									<template>
									<!-- <el-checkbox :id=i indeterminate="isIndeterminate" v-model="checkAll" @change="channge1(i)" :checked='test11' a='tttt'>全选</el-checkbox> -->
									<!-- <el-checkbox id="i" :v-model="checkAll7" @change="allchannge7" >全选</el-checkbox> -->
										<el-checkbox-group v-model="queryInfo.filter_list.resolution" @change="singleChange7">
											<el-checkbox v-for="(value,index) in dataList[7].list" :label="value" :key="value"  v-show="isOpen7|| index < Max">{{value}}</el-checkbox>
										</el-checkbox-group>
										<el-button v-show="!isOpen7 && dataList[7].list.length>Max" @click="isOpen7 =!isOpen7" type="text">展开 </el-button>
										<el-button v-show="isOpen7 && dataList[7].list.length>Max" @click="isOpen7 =!isOpen7"  type="text">收起 </el-button>
									</template>							
								</li>
							<li >
									{{dataList[8].title}}:  
									<template>
									<!-- <el-checkbox :id=i indeterminate="isIndeterminate" v-model="checkAll" @change="channge1(i)" :checked='test11' a='tttt'>全选</el-checkbox> -->
									<!-- <el-checkbox id="i" :v-model="checkAll8" @change="allchannge8" >全选</el-checkbox> -->
										<el-checkbox-group v-model="queryInfo.filter_list.state" @change="singleChange8">
											<el-checkbox v-for="value in dataList[8].list" :label="value" :key="value">{{value}}</el-checkbox>
										</el-checkbox-group>
									</template>							
								</li>	
							<li >
									{{dataList[9].title}}:  
									<template>
									<!-- <el-checkbox :id=i indeterminate="isIndeterminate" v-model="checkAll" @change="channge1(i)" :checked='test11' a='tttt'>全选</el-checkbox> -->
									<!-- <el-checkbox id="i" :v-model="checkAll8" @change="allchannge8" >全选</el-checkbox> -->
										<el-checkbox-group v-model="queryInfo.filter_list.position" @change="singleChange9">
											<el-checkbox v-for="value in dataList[9].list" :label="value" :key="value">{{value}}</el-checkbox>
										</el-checkbox-group>
									</template>							
								</li>																																																					
							</ul>
						</section>
						<div></div>
						<el-button type="warning"  @click="rest" size="mini" style="margin-left:25px;margin-top:0px" >重置</el-button>
					</div>
				</el-collapse-item>
			</el-collapse>	
			<!-- 设备列表区 -->
			<el-table :data="DevicesList" style="width: 100%" border stripe >
				<!-- 索引列 -->
				<el-table-column type="index" label="序号" width="50" align="center"></el-table-column>
				<!-- <el-table-column prop="mobile" label="测试" sortable align="center"></el-table-column>                -->
				<el-table-column prop="title" label="设备编号" sortable align="center" v-if="show"></el-table-column>
				<el-table-column prop="brand" label="品牌" sortable align="center" ></el-table-column>
				<el-table-column prop="name" label="设备名" sortable align="center"></el-table-column>
				<el-table-column prop="udid" label="udid" sortable align="center" v-if="show"></el-table-column>
				<el-table-column prop="resolution" label="分辨率" sortable align="center" ></el-table-column>
				<el-table-column prop="screen" label="屏幕类型" sortable align="center" ></el-table-column>
				<el-table-column prop="system" label="系统" sortable align="center"></el-table-column>
				<el-table-column prop="cpu" label="CPU" sortable align="center"></el-table-column>
				<el-table-column prop="gpu" label="GPU" sortable align="center" v-if="show"></el-table-column>
				<el-table-column prop="ram" label="内存" sortable align="center"></el-table-column>
				<el-table-column prop="platform" label="平台" sortable align="center" v-if="show"></el-table-column>
				<el-table-column  label="状态" align="center">
					<template slot-scope="scope">
						<el-tag type="sucess" round v-if="scope.row.state==='在库'" effect="plain">在库</el-tag>
						<el-tag type="info" round v-if="scope.row.state==='借出'"  effect="plain">借出</el-tag>
						<el-tag type="warning" round v-if="scope.row.state==='审批中'" effect="plain">审批中</el-tag>
						<el-tag type="danger" round v-if="scope.row.state==='锁定中'" effect="plain">锁定中</el-tag>
					</template>
				</el-table-column>
				<el-table-column prop="user_name" label="保管人" sortable align="center"></el-table-column>
				<el-table-column  label="设备信息修改" align="center">
					<template slot-scope="scope">
					<el-button type="primary" round @click="UpdateDevice(scope.row)" >修改</el-button>
					</template>
				</el-table-column>
			</el-table>
			<!-- 分页区 -->
			<!-- 可以根据数据库返回结果进行动态绑定 -->
			<el-pagination
				@size-change="handleSizeChange"
				@current-change="handleCurrentChange"
				:current-page="queryInfo.pagenum"
				:page-sizes="[10, 30, 60, 90, 300]"
				:page-size="queryInfo.pagesize"
				layout="total, sizes, prev, pager, next, jumper"
				:total="total"
			></el-pagination>
		</el-card>
		<!-- 信息弹出区 -->
		<el-dialog title="设备信息更改" :visible.sync="ShowDeviceInfoDialog" >
 			<el-form ref="AddNewDevices" :model="AddNewDevices" label-width="80px" style="width:100% margin:auto">
				<el-form-item label="设备编号">
					<el-input v-model="AddNewDevices.title"  style="width:25%"></el-input>
				</el-form-item>
				<el-form-item label="UDID">
					<el-input v-model="AddNewDevices.udid"  style="width:25%"></el-input>
				</el-form-item>
				<el-form-item label="设备名称">
					<el-input v-model="AddNewDevices.name"  style="width:15%"></el-input>
				</el-form-item>
				<el-form-item label="品牌">
					<el-input v-model="AddNewDevices.brand"  style="width:15%"></el-input>
				</el-form-item>
				<el-form-item label="CPU">
					<el-input v-model="AddNewDevices.cpu"  style="width:15%"></el-input>
				</el-form-item>
				<!-- <el-form-item label="GPU">
					<el-input v-model="AddNewDevices.gpu"  style="width:15%"></el-input>
				</el-form-item> -->
				<el-form-item label="系统">
					<el-input v-model="AddNewDevices.system"  style="width:15%"></el-input>
				</el-form-item>
				<el-form-item label="分辨率">
					<el-input v-model="AddNewDevices.resolution"  style="width:15%"></el-input>
				</el-form-item>
				<el-form-item label="屏幕类型">
					<el-select v-model="AddNewDevices.screen" placeholder="请选择屏幕类型">
						<el-option label="水滴屏" value="水滴屏"></el-option>
						<el-option label="曲面屏" value="曲面屏"></el-option>
						<el-option label="普通屏" value="普通屏"></el-option>
						<el-option label="刘海屏" value="刘海屏"></el-option>
						<el-option label="全面屏" value="全面屏"></el-option>
						</el-select>
				</el-form-item>
				<el-form-item label="运行内存">
					<el-input v-model="AddNewDevices.ram"  style="width:15%"></el-input>
				</el-form-item>
				<el-form-item label="平台">
					<el-select v-model="AddNewDevices.platform" placeholder="请选择平台">
						<el-option label="Android" value="android"></el-option>
						<el-option label="ios" value="ios"></el-option>
						</el-select>
				</el-form-item>                              
            <div style="text-align:center">
                <el-button type="primary"  round @click="submitForm">提交</el-button>
				<el-button type="primary" round @click="cancelsubmitForm" >取消</el-button>
            </div>
    	 	</el-form>			
		</el-dialog>
	</div>
</template>


<script>
export default {
	inject:['reload'],
    // 接口获取后台设备列表
	data() {
		return {
			//获取设备列表的参数对象
			//筛选的安卓&ios系统
			android:[],
			ios:[],
			queryInfo: {
				query: "",
				// 当前的页数
				pagenum: 1,
				// 当前每页显示多少条数据
				pagesize: 30,
				// 获取设备类型
                list_type: "",
                filter_list:{
					"brand":[],
					"system":[],
					"cpu":[],
					"gpu":[],
					"ram":[],
					"screen":[],
					"resolution":[],
					"state":[],
					"position":[],
				}
			},
			input: "",
			//保存获取的设备列表
			userList: [],
			total: 0,
			DevicesList: [],
            DevicesData: [],
			// 控制申请页面的隐藏
			AddSupplyDialog1: false,
			AddSupplyDialog2: false,
			//申请设备提交信息
			supplyform: {
				deviceid: "",
				reason: "",
				rtime: "",
				email: ""
			},
			formLabelWidth: "120px",
			// DateRules:{
			//     date:{ validator: checkdate, trigger: 'blur' }
			// }
			dataList:null,
			show :false,
			show1:false,
			show2:false,
			show3:false,
			activenames:['1'],
			// 修改设备面板
			AddNewDevices: {
				id:'',
				title: "",
				udid: "",
				name: "",
				brand: "",
				cpu: "",
				gpu: "",
				system: "",
				resolution: "",
				screen: "",
				ram: "",
				platform: ""
			  },
			ShowDeviceInfoDialog:false,
			// 筛选标签最大显示量
			Max:8,
			isOpen :false,
			isOpen1 :false,
			isOpen2 :false,
			isOpen3 :false,
			isOpen31 :false,
			isOpen32 :false,
			isOpen33 :false,
			isOpen4 :false,
			isOpen41 :false,
			isOpen42 :false,
			isOpen43 :false,		
			isOpen5 :false,
			isOpen6 :false,
			isOpen7 :false,
			
        };
	},
	created() {
		//获取设备列表
		// this.getUserList();
		this.getDevicesList();
		// 获取筛选列表
		this.getFilter();
	},
	methods: {
		// 请求后台参数,并获取设备列表
		async getDevicesList() {
			// 获取设备信息、每条显示数、每页显示数
			const { data: res } = await this.$http.get("equip_list", {
				params: this.queryInfo
			});
			if (res.meta.status !== 200) {
				return this.$message.error(res.meta.msg);
			}
			// 后台获取的设备列表渲染到前台
			this.DevicesList = res.data.dict.list;
			this.total = res.data.dict.total;
		},
		//监听pagesize改变
		handleSizeChange(newSize) {
			console.log(newSize);
			this.queryInfo.pagesize = newSize;
			this.getDevicesList();
		},
		//监听页码改变
		handleCurrentChange(newPage) {
			// console.log(newPage)
			this.queryInfo.pagenum = newPage;
			this.getDevicesList();
		},
		//搜索事件
		Search() {
			// console.log(this.queryInfo.query)
			this.getDevicesList();
		},
		//显示所有信息
		showAllinfo(){
			this.show = !this.show
		},
		// 修改设备
		UpdateDevice(data){
			console.log(data)
			this.AddNewDevices.id = data.id
			this.AddNewDevices.title = data.title
			this.AddNewDevices.udid = data.udid
			this.AddNewDevices.name = data.name
			this.AddNewDevices.brand = data.brand
			this.AddNewDevices.cpu = data.cpu
			this.AddNewDevices.gpu = data.gpu
			this.AddNewDevices.system = data.system
			this.AddNewDevices.resolution = data.resolution
			this.AddNewDevices.screen = data.screen
			this.AddNewDevices.ram = data.ram
			this.AddNewDevices.platform = data.platform
			this.ShowDeviceInfoDialog=true
	

		},
		// 提交设备修改信息
		async submitForm() {
      		const { data: res } = await this.$http.get("equip_update", {
        	params: this.AddNewDevices
      		});
      		if (res.meta.status !== 200) {
				if(res.meta.status == 401){
					this.$message.error("你还不是管理员");
					return this.$router.push('./login')
				}
        	return this.$message.error("修改设备失败");
      	}
			this.$message.success("修改设备成功");
			this.ShowDeviceInfoDialog=false,
			this.getDevicesList()

    	},
		// 取消提交设备修改信息
		cancelsubmitForm(){
			this.ShowDeviceInfoDialog = false
		},
//筛选方法
		singleChange0(value) {
			if(this.queryInfo.filter_list.brand.length==0){
				this.getDevicesList()
				return this.show2 = this.show3 =false

			}
			if(this.queryInfo.filter_list.brand == "苹果"){
				this.show2 = true
				}else{
					this.show2 = false
				}
			if(this.queryInfo.filter_list.brand.length!=0){
				if(this.queryInfo.filter_list.brand.indexOf("苹果") == -1 ){
					this.show3 =true
				}else{
					this.show3 = false
				}
			}
			this.queryInfo.pagenum=1		
			this.getDevicesList();
		},
		singleChange1(value) {
			this.queryInfo.filter_list.system = this.android.concat(this.ios)
			this.queryInfo.pagenum=1
			this.getDevicesList();			
		},
		singleChange2(value) {
			this.queryInfo.filter_list.system = this.android.concat(this.ios)
			this.queryInfo.pagenum=1
			this.getDevicesList();			
		},				  
		singleChange3(value) {
			// let checkedCount = value.length;
			// this.checkAll3 = checkedCount === this.dataList[3].list.length;
			this.queryInfo.pagenum=1
			this.getDevicesList();			
		},
		singleChange4(value) {
			// let checkedCount = value.length;
			// this.checkAll4 = checkedCount === this.dataList[4].list.length;
			this.queryInfo.pagenum=1
			this.show1 = true
			this.getDevicesList();			
		},
		singleChange5(value) {
			// let checkedCount = value.length;
			// this.checkAll5 = checkedCount === this.dataList[5].list.length;
			this.queryInfo.pagenum=1
			this.getDevicesList();			
		},
		singleChange6(value) {
			// let checkedCount = value.length;
			// this.checkAll6 = checkedCount === this.dataList[6].list.length;
			this.queryInfo.pagenum=1
			this.getDevicesList();			
		},

		singleChange7(value) {
			// let checkedCount = value.length;
			// this.checkAll7 = checkedCount === this.dataList[7].list.length;
			this.queryInfo.pagenum=1
			this.getDevicesList();			
		},
		// allchannge8(val) {
		// 	this.queryInfo.filter_list.state = val ? this.dataList[8].list : [];
		// 	this.queryInfo.pagenum=1
		// 	this.getDevicesList();			
		// },
		singleChange8(value) {
			// let checkedCount = value.length;
			// this.checkAll8 = checkedCount === this.dataList[8].list.length;
			this.queryInfo.pagenum=1
			this.getDevicesList();			
		},
		singleChange9(value) {
			// let checkedCount = value.length;
			// this.checkAll8 = checkedCount === this.dataList[9].list.length;
			this.queryInfo.pagenum=1
			this.getDevicesList();			
		},
		// 重置筛选(刷新页面)
		rest(){
				  this.reload()	},
		async getFilter(){
			const{data:res} = await this.$http.get("get_fl_list");
			this.dataList = res.data.dict.DeviceType
			if(res.meta.status !==200){
				return this.$message.error(res.meta.msg)
			}
		},
	}
};
</script>


<style lang="less" scoped>
.nav {
	margin-bottom: 10px;
}
.button {
	margin: 10px;
}
#showinfo{
	float:right;
	align-items: center;
}
.el-checkbox-group{
	display: inline;
}
.el-checkbox{
	display:inline-block
}
#i{
	margin-right: 20px
}
#choice {
    width: 100%;
	height: auto;
	padding-top:0px ;
	margin-top: 0px;
	// background: url(img/bg.jpg) no-repeat 0 0;
	display:inline-block
}
#section {
    width: 100%px;
    height: auto;
	margin-bottom: 0px;
	padding-top:0px ;
	display:inline-block
}
#type {
    width: 100%;
    height: auto;
    // background: url(img/type_bg.png) no-repeat 0 0;
    padding: 17px 0 16px 28px;
	margin-top: 0 ;
	display:inline-block
	
}
#type li {
    height: auto;
    color: #8a8a8a;
	line-height: 35px;
	list-style-type:none

}

.active {
    background:   #00ff00
}

</style>>
