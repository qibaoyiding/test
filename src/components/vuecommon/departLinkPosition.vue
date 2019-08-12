<template>
	<div class="col-xs-12 col-sm-12 col-md-12 col-lg-12">
		<div class="col-xs-3 col-sm-3 col-md-3 col-lg-3">
			<div class="col-xs-2 col-sm-2 col-md-2 col-lg-2" style="padding: 0; line-height: 34px;">
				<span class="countdate">部门：</span>
			</div>
			<div class="col-xs-10 col-sm-10 col-md-10 col-lg-10">
				<depart @departChange='departChange'></depart>
			</div>
		</div>
		<div class="col-xs-2 col-sm-2 col-md-2 col-lg-2">
			<div class="col-xs-3 col-sm-3 col-md-3 col-lg-3" style="padding: 0; line-height: 34px;">
				<span class="countdate">岗位：</span>
			</div>
			<div class="col-xs-9 col-sm-9 col-md-9 col-lg-9" style="padding: 0; line-height: 34px;">
				<select class="form-control" v-model.lazy="positionId" v-on:change="positionChange()">
					<option value="0">---请选择---</option>
					<option v-for="(item,index) in positionList" :key="index" v-bind:value="item.position_ID">{{item.position_Name}}</option>
				</select>
			</div>	
		</div>	
		
	</div>
</template>

<script>
	import axios from "axios";
	import depart from '../vuecommon/department.vue'
	import position from '../vuecommon/position.vue'
	export default {
		name: "deptchange",
		components: {
			depart,
			position
		},
		data() {
			return {
				departName: "",
				departId: "0",
				positionId: '0',
				positionName: '',
				positionList: [],
			};
		},
		methods: {
			departChange: function(departId, departName) {
				this.departLinkChange(departId)
				this.$emit('departChange',departId,departName)
				this.positionId='0'
			},
			positionChange: function() {
				this.$emit('positionChange', this.positionId)
			},
			//岗位随部门ID联动
			departLinkChange(departId) {
				var url = this.url + "/kqParamSetContr/queryDepartmentPosition";
				axios({
						method: "post",
						url: url,
						headers: {
							"Content-Type": this.contentType,
							"Access-Token": this.accessToken
						},
						data: {
							deptId: departId
						},
						dataType: "json"
					})
					.then(response => {
						if (response.data.retCode == "0000") {
							this.positionList = response.data.retData
						} else {
							console.log(response.data);
						}
					})
					.catch(function(error) {
						console.log("请求失败处理");
					});
			},
		}
	};
</script>

<style>
</style>
