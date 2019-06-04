<template>
	<view>
		<inputs
			:inputsArray="inputsArray"
			activeName="提交"
			:ruleSet="ruleSet"
			ifRule
			ifCode
			@chaildOpenEvent="openWin"
			@activeFc="activeFc"
			:onLoadData="onLoadData"
			animationType="rotate3d-fade"
			:animationDuration="0.1"
			submitReSet
			:buttonStyle="buttonStyle"
			:inputDebounceSet="inputDebounceSet"
		/>

		<!-- <button type="primary" @tap="openTest()" style="margin-top: 50px;">打开test页面</button> -->
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
					segmentationTitle: '填写个人资料', //分割大标题
					border_top: '1px solid #f2f2f2', //上划线
					title: '姓名',
					ignore: false, //是否可忽略该值(判断时此项值可以为空)
					// defaultValue: '今天也要加油鸭~',
					tapClear: true, //input一键清除功能
					password: false, //input密码类型
					// icon: 'search', //input左边图标
					// iconColor: '#c0ebd7', //input图标颜色
					// filterFc: function(value) {
					// 	//input值过滤函数
					// 	//自定义过滤函数
					// 	value = 'filter过滤后的值';
					// 	return value;
					// },
					variableName: 'name' //自定义变量名
				},
				{
					title: '性别',
					type: 'radio',
					itemArray: [
						{
							//子循环数组
							name: '男',
							value: '1',
							defaultValue: true
						},
						{
							name: '女',
							value: '2'
						}
					],
					color: '#c0ebd7',
					scale: '.8', //比例大小
					variableName: 'sex' //自定义变量名
				},
				{
					type: 'pics',
					title: '照片',
					itemArray: [
						{
							title: '照片',
							ignore: true
						}
					],
					variableName: 'pic',
					clearColor: '#c0ebd7',
					ignore: 'false',
					variableName: 'photo' //自定义变量名
				},
				{
					title: '手机号',
					phone: true,
					tapClear: true, //input一键清除功能
					// defaultValue: '13305679845'
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
			console.log(res);
			uni.request({
				url: uni.getStorageSync('tempUrl') + 'visitor/insertSelective',
				data: {
					name: res.name,
					phone: res.phone,
					photo: res.photo,
					sex: res.sex,
					wechatOpenid: uni.getStorageSync('openId')
				},
				method: 'POST',
				success: res => {
					console.log(res);
					if (res.data.code === 0) {
						uni.navigateTo({
							url: './visitorApply'
						});
					} else {
						alert('失败');
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
