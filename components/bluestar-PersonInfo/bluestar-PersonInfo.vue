<template>
	<view class="person-info">
		<view class="top-area">
			<view
				:class="['user-avatarUrl' , perInfo.value.sex === '1' ? 'nan' : perInfo.value.sex === '2' ? 'nv' : '']"></view>
			<view class="info">
				<view class="name">{{ perInfo.value.username }}</view>
				<view class="age">{{ perInfo.value.sex === '1' ? '男' : perInfo.value.sex === '2' ? '女' : '' }}
					{{ perInfo.value.age + '岁' }}
				</view>
			</view>
			<view class="per-change">
				<picker @change="perSelect"
								:range="actions0.value"
								:range-key="'username'">
					<view class="change">切换用户</view>
				</picker>
			</view>
		</view>
		<view class="org-change" v-if="isOrg">
			<picker @change="orgSelect"
							:range="actions1.value"
							:range-key="'orgname'">
				<view class="organ-area">当前机构：{{ Object.values(orgInfo.value).length > 0 ? orgInfo.value.orgname : '' }}</view>
			</picker>
		</view>
	</view>
</template>

<script setup lang="ts">
import {onMounted, reactive, toRaw, withDefaults} from "vue"
import {getHospitalInfo} from "@/common/api/index"
import utils from "@/common/ts/utils"

let index0 : number = 0
let actions0 : any = reactive({
	value : [
		{
			username : '张三',
			sex : '2',
			age : '24'
		}, {
			username : '李四',
			sex : '1',
			age : '32'
		}]
})
let index1 : number = 0
let actions1 : any = reactive({value : {}})

const perInfo : any = reactive({value : {}})
const orgInfo : any = reactive({value : {}})

interface Props {
	isOrg? : boolean
}

const props = withDefaults(defineProps<Props>(), {
	isOrg : false,
})

const emit = defineEmits<{
	(e : 'init', perInfo : any, orgInfo : any) : void
}>()

onMounted(() => init())

const init = async () => {
	//用户
	perInfo.value = Object.assign({}, actions0.value[index0])

	//机构
	if (props.isOrg) {
		let res = await getHospitalInfo(Object.assign({}, {
			areacode : utils.bodyParams.req_data.vercode,
		}))
		if (res.code === '200') {
			actions1.value = res.baseinfo
			orgInfo.value = Object.assign({}, actions1.value[index1])
		}
	}
	emit('init', toRaw(actions0.value[index0]), toRaw(actions1.value[index1]))
}

//人员切换
const perSelect = (event : any) => {
	index0 = event.detail.value * 1
	perInfo.value = Object.assign({}, actions0.value[index0])
	emit('init', toRaw(actions0.value[index0]), toRaw(actions1.value[index1]))
}

//机构切换
const orgSelect = (event : any) => {
	index1 = event.detail.value * 1
	orgInfo.value = Object.assign({}, actions1.value[index1])
	emit('init', toRaw(actions0.value[index0]), toRaw(actions1.value[index1]))
}
</script>

<style lang="scss" scoped>
.person-info {
	width: 100%;
	padding: vw(40) vw(20);
	background-color: #ffffff;
	box-shadow: 0 0 vw(12) rgba(61, 156, 253, 0.26);
	border-radius: vw(20);
	.top-area {
		display: flex;
		align-items: center;
		.user-avatarUrl {
			width: vw(80);
			height: vw(80);
			margin: 0 vw(20) 0 0;
			background-repeat: no-repeat;
			background-position: 0 0;
			background-size: 100% 100%;
			&.nan {
				background-image: url("./nan@3x.png");
			}
			&.nv {
				background-image: url("./nv@3x.png");
			}
		}
		.info {
			height: vw(80);
			display: flex;
			flex-flow: column nowrap;
			justify-content: space-between;
			.name {
				font-size: vw(30);
				font-weight: bold;
				color: #333333;
			}
			.age {
				font-size: vw(24);
				color: #555555;
			}
		}
		.per-change {
			flex: 1;
			display: flex;
			justify-content: flex-end;
			align-items: center;
			.change {
				height: vw(40);
				line-height: vw(40);
				background: $base-color url("./qhjr_icon@3x.png") no-repeat vw(20) center / vw(20) vw(10);
				padding: 0 vw(20) 0 vw(55);
				border-radius: vw(18);
				font-size: vw(22);
				color: #FFFFFF;
			}
		}
	}
	.org-change {
		width: 100%;
		height: vw(70);
		line-height: vw(70);
		margin: vw(40) 0 0 0;
		background: $base-color url("./jr_w_icon@3x.png") no-repeat 95% center / vw(12) vw(21);
		border-radius: vw(35);
		font-size: vw(24);
		color: #ffffff;
		text-align: center;
	}
}
</style>
