<template>
	<div class="tab-pane fade in active martop" id="basic">
		<form action="">
			<div class="form-group clearfix">
				<label for="cyname" class="col-md-2 control-label text-right nopad">姓　　名：</label>
				<div class="col-md-5">
					<input type="text" class="form-control" v-model="personalBase.name" placeholder="">
				</div>
			</div>
			<div class="form-group clearfix">
				<label for="sex" class="col-md-2 control-label text-right nopad">性　　别：</label>
				<div class="col-md-5">
					<input type="text" class="form-control" v-model="personalBase.sex" placeholder="">
				</div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">出生年月：</label>
				<div class="col-md-5 input-group date form_date">
					<input type="date" class="form-control" v-model="personalBase.birth" value="">
					<span class="input-group-addon"><span class="glyphicon glyphicon-calendar"></span></span> </div>
			</div>
			<div class="form-group clearfix">
				<label for="gh" class="col-md-2 control-label text-right nopad">工　　号：</label>
				<div class="col-md-5">
					<input type="text" class="form-control" v-model="personalBase.jobNum" placeholder="">
				</div>
			</div>
			<div class="form-group clearfix">
				<label for="erpzh" class="col-md-2 control-label text-right nopad">ERP账号：</label>
				<div class="col-md-5">
					<input type="text" class="form-control" v-model="personalBase.erpaaccount" placeholder="">
				</div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">部　　门：</label>
				<div class="col-md-5">
					<department @departChange='departChange'></department>
				</div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">职　　位：</label>
				<div class="col-md-5">
					<position @positionChange='positionChange'></position>
				</div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">入职日期：</label>
				<div class="col-md-5 input-group date form_date">
					<input type="date" class="form-control" v-model="personalBase.entryDate" value="">
					<span class="input-group-addon"><span class="glyphicon glyphicon-calendar"></span></span> </div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">转正日期：</label>
				<div class="col-md-5 input-group date form_date">
					<input type="date" class="form-control" v-model="personalBase.positiveDate" value="">
					<span class="input-group-addon"><span class="glyphicon glyphicon-calendar"></span></span> </div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">离职日期：</label>
				<div class="col-md-5 input-group date form_date">
					<input type="date" class="form-control" v-model="personalBase.resignDate" value="">
					<span class="input-group-addon"><span class="glyphicon glyphicon-calendar"></span></span> </div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">离职类型：</label>
				<div class="col-md-5 input-group date form_date">
					<select class="form-control" >
						<option></option>
					</select>
				</div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">离职原因：</label>
				<div class="col-md-5 input-group date form_date">
					<select class="form-control" >
						<option></option>
					</select>
				</div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">状　　态：</label>
				<div class="col-md-5 input-group date form_date">
					<select class="form-control" v-model="personalBase.state">
						<option value="1">在职</option>
						<option value="2">离职</option>
					</select>
				</div>
			</div>
			<div class="form-group clearfix">
				<label class="col-md-2 control-label text-right nopad">备　　注：</label>
				<div class="col-md-5">
					<textarea class="form-control" rows="3" v-model="personalBase.remark"></textarea>
				</div>
			</div>
		</form>
	</div>
	
</template>

<script>
	import department from '../../../vuecommon/department.vue'
	import position from '../../../vuecommon/position.vue'
	export default {
		components:{department,position},
		data() {
			return {
				personalBase:{},
				
				departmentList:[],
				positionList:[],
			};
		},
		methods:{
			//获取部门名字和id
			departChange: function(departId, departName) {
				
				this.personalBase.departId = departId
				this.personalBase.departName = departName
			},
			positionChange:function (positionId,positionName){
				
				this.personalBase.positionId = positionId
				this.personalBase.positionName = positionName
				
			}
		}
	}
</script>

<style>

</style>
