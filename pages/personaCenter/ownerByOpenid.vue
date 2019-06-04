<template>
	<view>
		<inputs
			:inputsArray="inputsArray"
			activeName="提交"
			:ruleSet="ruleSet"
			ifRule
			@chaildOpenEvent="openWin"
			@activeFc="activeFc"
			:onLoadData="onLoadData"
			animationType="rotate3d-fade"
			:animationDuration="0.1"
			submitReSet
			:buttonStyle="buttonStyle"
			:inputDebounceSet="inputDebounceSet"
		/>
	</view>
</template>

<script>
import inputs from '@/components/QuShe-inputs/inputs.vue';
export default {
	data() {
		return {
			inputDebounceSet: {
				openInputDebounce: true,
				delay: 500
			},
			buttonStyle: {
				//按钮样式
				activeButton: 'background-color: #c0ebd7;border-radius: 30px;box-shadow: 2px 2px 1px 1px #c0ebd7;', //主按钮样式
				changeButton: 'background-color: #c0ebd7;border-radius: 30px;box-shadow: 2px 2px 1px 1px #c0ebd7;', //picker类型更改按钮样式
				selectButton: 'background-color: #c0ebd7;border-radius: 30px;box-shadow: 2px 2px 1px 1px #c0ebd7;', //picker类型选择按钮样式
				confirmButton: 'background-color: #c0ebd7;border-radius: 30px;box-shadow: 2px 2px 1px 1px #c0ebd7;', //picker类型弹出框中确定按钮样式
				getcodeButton: 'background-color: #c0ebd7;border-radius: 30px;box-shadow: 2px 2px 1px 1px #c0ebd7;' //获取验证码按钮样式
			},
			inputsArray: [
				{
					segmentationTitle: '尊敬的业主，您好', //分割大标题
					border_top: '1px solid #f2f2f2', //上划线
					type: 'text',
					// title: 'text示例',
					content: '请先前往物业处完成登记后再绑定',
					ellipsis: true
				},
				{
					segmentationTitle: '完善信息', //分割大标题
					border_top: '1px solid #f2f2f2', //上划线
					title: '手机号',
					verifyFc: function(value) {
						if (/^[1][3,4,5,7,8][0-9]{9}$/.test(value)) return true;
						return false;
					},
					verifyErr: '手机号校验错误',
					variableName: 'phone' //自定义变量名
				}
			],
			ruleSet: {
				color: '#c0ebd7',
				checkbox_color: '#c0ebd7',
				itemArray: [
					{
						name: '某规则',
						value: 'aa'
					}
				]
			},
			onLoadData: 'data_'
		};
	},
	onLoad() {},
	methods: {
		openWin(value) {
			//打开规则或协议页
			//若有一个以上的rule，则根据value打开规则页面
			console.log(value);
		},
		activeFc(res) {
			uni.showToast({
				title: '获取输入成功'
			});
			uni.request({
				url: uni.getStorageSync('tempUrl') + 'owner/updateByPhone',
				data: {
					phone: res.phone,
					wechatOpenid: uni.getStorageSync('openId')
				},
				method: 'PUT',
				success: res => {
					console.log(res);
					if (res.data.code === 0) {
						alert('成功啦');
						uni.navigateBack({
							delta: 1
						});
					} else {
						alert('失败了，请先前往物业登记');
					}
				}
			});
		}
	},
	components: {
		inputs
	}
};
</script>

<style></style>
