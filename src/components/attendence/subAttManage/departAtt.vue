<template>
	<div class="tab-pane fade" id="bmcqmx">
		<div class="col-xs-12 col-sm-12 col-md-12 col-lg-12">
			<div class="table-responsive" id="common-app">
				<div class="col-xs-10 col-sm-10 col-md-10 col-lg-10 mtr_a">
					
					<span>部门：</span> 
					<span class="com-sel">
						<depart :departName="departName" :departId="departId" @departChange='departChange'></depart>
					</span>
					 <span>时间：</span> <span>
						<input type="date" value="" class="form-control" v-model="dpBeginDate" />
					</span> 
					<span>&nbsp;&nbsp;&nbsp;至：</span> <span>
						<input type="date" value="" class="form-control" v-model="dpEndDate" />
					</span>
					 <span class="search">
						<button class="btn btn-warning" v-on:click="searchDepartKQInfo('0')">查询所有</button>
					</span>
					 <span class="search">
						<button class="btn btn-warning" v-on:click="searchDepartKQInfo('1')">查询</button>
					</span> 
					<span class="search">
						<button class="btn btn-primary" @click="exportTableToExcel('deptAttTB','部门出勤统计')">导出</button>
					</span> </div>
				<div class="col-lg-11 mtr_a"> <span>注：</span> <span style="color:#FF0000; margin-right:10px;">旷工</span> <span style="color:#CD853F; margin-right:10px;">迟到</span>
					<span style="color:#000000; margin-right:10px;">正常</span>
					<span style="color:#9370DB; margin-right:10px;">休息</span> <span style="color:#006400; margin-right:10px;">请假</span>
					<span style="color:#00679D; margin-right:10px;">倒休</span> <span style="color:#FF7F50; margin-right:10px;">打卡异常</span>
					<span style="color:#87CEFA; margin-right:10px;">加班</span> <span style="color:#D2B48C; margin-right:10px;">漏打卡</span>
				</div>
				<table class="table table-bordered table-hover" id='deptAttTB'>
					<thead>
						<tr>
							<th class="text-center">部门</th>
							<th class="text-center">迟到次数</th>
							<th class="text-center">早退次数</th>
							<th class="text-center">旷工次数</th>
							<th class="text-center">上班打卡异常</th>
							<th class="text-center">下班打卡异常</th>
							<th class="text-center">休息天数</th>
							<th class="text-center">加班天数</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(item,index) in departKqList" :key="index" @dblclick="showSingleDepartAttend(item)">
							<td>{{item.departKQName}}</td>
							<td>{{item.laterTimes}}</td>
							<td>{{item.leaveEarlyTimes}}</td>
							<td>{{item.minersTimes}}</td>
							<td>{{item.onPA}}</td>
							<td>{{item.downPA}}</td>
							<td>{{item.restDays}}</td>
							<td>{{item.overTimesDays}}</td>
						</tr>
					</tbody>
				</table>
			</div>
		</div>
		<singleDepart></singleDepart>
	</div>

</template>

<script>
	import axios from 'axios'
	import {
		timeInit
	} from '../../../assets/js/date.js'
	import depart from '../../vuecommon/department.vue'
	import singleDepart from './singleDepartAtt.vue'

	export default {
		components: {
			depart,
			singleDepart
		},
		data() {
			return {
				departId: '0',
				departName: '0',
				dpBeginDate: timeInit(''),
				dpEndDate: timeInit(''),
				departKqList: [],
			};
		},
		methods: {
			//获取部门名字和id
			departChange: function(departId, departName) {
				this.departId = departId
				this.departName = departName
			},
			//单个部门人员考勤信息
			showSingleDepartAttend(param) {
				param.departName = param.departKQName
				param.beginDate = this.dpBeginDate
				param.endDate = this.dpEndDate
				this.$children[1].singleDepartAttend(param)
				$("#myDepartmentAttendance").modal('show')

			},
			//部门考勤信息汇总
			searchDepartKQInfo: function(param) {

				if (param == '0') {
					this.departName = ""
					this.$children[0].departId = '0'
				} else {
					if (this.departId == '0') {
						this.departName = ""
					}
				}
				// alert(this.departName)
				var url = this.url + '/kqgl/searchDepartKQList'
				// alert(url)
				axios({
					method: 'post',
					url: url,
					headers: {
						'Content-Type': this.contentType,
						'Access-Token': this.accessToken
					},
					data: {
						positionName: "",
						name: "",
						jobNum: "",
						departName: this.departName,
						beginDate: this.dpBeginDate,
						endDate: this.getYYYYMMDDHHMMSS_24(this.dpEndDate),
					},
					dataType: 'json',
				}).then((response) => {
					var res = response.data
					console.log('departKqList')
					if (res.retCode == '0000') {
						console.log('departKqList')
						if (res.resData.length > 0) {
							console.log('departKqList-length:' + res.resData.length)
							this.departKqList = res.resData
						} else {
							alert('没有查询到相关数据')
						}
					} else {
						alert(res.retMsg)
					}
				}).catch((error) => {
					console.log('请求失败')
				});
			},
			//获取部门考勤
			async getDepartKqList() {

				var url = this.url + '/kqgl/searchDepartKQList'
				// alert(url)
				axios({
					method: 'post',
					url: url,
					headers: {
						'Content-Type': this.contentType,
						'Access-Token': this.accessToken
					},
					data: {
						departName: "",
						positionName: "",
						name: "",
						jobNum: "",
						beginDate: this.dpBeginDate,
						endDate: this.getYYYYMMDDHHMMSS_24(this.dpEndDate),
					},
					dataType: 'json',
				}).then((response) => {
					var res = response.data
					if (res.retCode == '0000') {
						if (res.resData.length > 0) {
							console.log('departKqList-length:' + res.resData.length)
							this.departKqList = res.resData
						} else {
							alert('没有查询到相关数据')
						}
					} else {
						alert(res.retMsg)
					}
				}).catch((error) => {
					console.log('请求失败')
				});
			}
		},
		created() {
			this.getDepartKqList()
		}
	}
</script>

<style>

</style>
