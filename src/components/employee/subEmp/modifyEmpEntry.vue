<template>
	<!--修改员工弹出-->
	<div class="modal-content">
		<div class="modal-header">
			<button type="button" data-dismiss="modal" aria-hidden="true" class="close">×</button>
			<h4 id="myModalLabel" class="modal-title">员工信息修改</h4>
		</div>
		<div class="modal-body">
			<ul class="nav nav-tabs martop">
				<li class="active"><a href="#updatabasic" data-toggle="tab">基本信息</a></li>
				<li><a href="#updatadetailed" data-toggle="tab">详细信息</a></li>
				<li><a href="#updatafamily" data-toggle="tab">家庭成员</a></li>
				<li><a href="#updatarecord" data-toggle="tab">调动记录</a></li>
				<li class="pull-right" id="lookms"><a href="" data-toggle="tab">查看面试</a></li>
			</ul>
			<div class="tab-content" style=" height:600px; overflow-y:scroll;">
				<!-- 人员基本信息 -->
				<baseInfo ref='baseInfo'></baseInfo>
				<!-- 人员详细信息 -->
				<detailInfo ref='detailInfo'></detailInfo>
				<!--家庭成员-->
				<familyInfo ref='familyInfo'></familyInfo>
				<!--调动记录-->
				<shiftInfo ref='shiftInfo'></shiftInfo>
			</div>
		</div>
		<div class="modal-footer">
			<!--按钮-->
			<div class="col-md-12">
				<div class="col-md-8">
					<button type="button" class="btn btn-info btn-sm int_pwd" v-on:click="initPassword()">初始化密码</button>
					<button type="button" class="btn btn-info btn-sm reg_work" id="shiftPositionBtn" v-if="isShiftPosition==true" v-on:click="shiftPosition()">转正</button>
					<button type="button" class="btn btn-info btn-sm recovery" v-on:click="deleteEmployee()">离职</button>
					<button type="button" class="btn btn-info btn-sm detl" v-on:click="employeeResgin()">删除</button>
				</div>
				<button type="button" class="btn btn-info" v-on:click="updataEmployeeInfo()">确认</button>
				<button type="button" data-dismiss="modal" class="btn btn-info">返回</button>
			</div>
		</div>
	</div>


</template>

<script>
	import axios from 'axios'
	import baseInfo from '../subEmp/subModifyEE/modifyEmpBase.vue'
	import detailInfo from '../subEmp/subModifyEE/modifyEmpDetail.vue'
	import familyInfo from '../subEmp/subModifyEE/modifyEmpFamily.vue'
	import shiftInfo from '../subEmp/subModifyEE/modifyEmpShift.vue'
	export default {
		components: {
			baseInfo,
			detailInfo,
			familyInfo,
			shiftInfo,
		},
		
		data() {
			return {
				personalBase:{},
				personalDetail:{},
				personalFamily:[],
				
				personalShift:[],
				
				isShiftPosition:true,
				
				accountId:'',
				userId:'',
			};
		},
		methods: {
			//传对应的参数给子模块
			paramDevliverToSubModel:function(param){
				this.personalBase = param
				this.accountId = param.accountId
				this.userId = param.id
				
				//判断是否显示转正按钮
				if(this.isBlank(this.personalBase.positiveDate)){
					this.isShiftPosition=true
				}else{
					this.isShiftPosition=false
				}
				console.log('isShiftPosition'+this.isShiftPosition)
				this.$refs.baseInfo.childrenBaseInfo(this.personalBase)
				this.$refs.detailInfo.childrenDetailInfo(this.userId)
				this.$refs.familyInfo.childrenFamilyInfo(this.accountId)
				this.$refs.shiftInfo.childrenShiftInfo(this.accountId)
				// if(this.personalBase.entryDate<)
			},
			//初始化密码
			initPassword:function(){
				
				var url = this.url + '/search/updataAccountPwd'
				axios({
					method: 'post',
					url: url,
					headers: {
						'Content-Type': this.contentType,
						'Access-Token': this.accessToken
					},
					data: {
						accountId: this.accountId,
					},
					dataType: 'json',
				}).then((response) => {
					var res = response.data
					console.log(res)
					if (res.retCode == '0000') {
						alert('密码初始化成功')
					} else {
						alert(res.retMsg)
					}
				
				}).catch((error) => {
					console.log('请求失败处理')
				});
			},
			//转正
			shiftPosition:function(){
				var url = this.url + '/search/shiftPositiveDate'
				axios({
					method: 'post',
					url: url,
					headers: {
						'Content-Type': this.contentType,
						'Access-Token': this.accessToken
					},
					data: {
						id: this.userId,
						positiveDate:this.personalBase.positiveDate
					},
					dataType: 'json',
				}).then((response) => {
					var res = response.data
					console.log('转正:'+res)
					if (res.retCode == '0000') {
						this.submitBackUpPage()
					} else {
						alert(res.retMsg)
					}
				
				}).catch((error) => {
					console.log('请求失败处理')
				});
			},
			//离职
			employeeResgin:function(){
				var url = this.url + '/search/employeeResginDate'
				axios({
					method: 'post',
					url: url,
					headers: {
						'Content-Type': this.contentType,
						'Access-Token': this.accessToken
					},
					data: {
						id: this.userId,
						resignDate:this.personalBase.resignDate
					},
					dataType: 'json',
				}).then((response) => {
					var res = response.data
					console.log(res)
					if (res.retCode == '0000') {
						this.submitBackUpPage()
					} else {
						alert(res.retMsg)
					}
				}).catch((error) => {
					console.log('请求失败处理')
				});
			},
			//删除
			deleteEmployee:function(){
				
				var url = this.url + '/search/deleteEmployee'
				axios({
					method: 'post',
					url: url,
					headers: {
						'Content-Type': this.contentType,
						'Access-Token': this.accessToken
					},
					data: {
						id: this.userId,
						idDelete:'1'
					},
					dataType: 'json',
				}).then((response) => {
					var res = response.data
					console.log(res)
					if (res.retCode == '0000') {
						this.submitBackUpPage()
					} else {
						alert(res.retMsg)
					}
				
				}).catch((error) => {
					console.log('请求失败处理')
				});
			},
			
			// 提交修改的员工信息
			updataEmployeeInfo: function() {
				// 修改基本信息
				this.personalBase = this.$refs.baseInfo.personalBase
				this.personalDetail = this.$refs.detailInfo.personalDetail
				this.personalFamily = this.$refs.familyInfo.personalFamily
				
				var userInfo = this.personalDetail
				userInfo = this.personalBase
				
				console.log(userInfo)
				var url = this.url + '/search/updateUserInfo'
				
	
				axios({
					method: 'post',
					url: url,
					headers: {
						'Content-Type': this.contentType,
						'Access-Token': this.accessToken
					},
					data: {
						//基本信息
						userBase: this.personalBase,
						//详细信息
						userDetail:this.personalDetail,
						//家庭信息
						userFamily: this.personalFaminly,
					},
					dataType: 'json',
				}).then((response) => {
					var res = response.data
					console.log(res)
					if (res.retCode == '0000') {
						alert(res.resData.message)
						this.submitBackUpPage()
					} else {
						alert(res.retMsg)
					}

				}).catch((error) => {
					console.log('请求失败处理')
				});
			},
			submitBackUpPage:function(){
				this.$emit('submitBackUpPage')
			}
		}

	}
</script>

<style>

</style>
