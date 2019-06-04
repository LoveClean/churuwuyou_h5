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
					segmentationTitle: '报修申请', //分割大标题
					border_top: '1px solid #f2f2f2', //上划线
					title: '标题',
					ignore: true, //是否可忽略该值(判断时此项值可以为空)
					// defaultValue: '今天也要加油鸭~',
					tapClear: true, //input一键清除功能
					variableName: 'title' //自定义变量名
				},
				{
					type: 'textarea',
					title: '内容',
					width: '100%',
					height: '100',
					maxlength: '-1',
					// defaultValue: '今天也要加油鸭~', //默认值
					variableName: 'content' //自定义变量名
				}
				// {
				// 	type: 'pics',
				// 	title: '图片',
				// 	itemArray: [
				// 		{
				// 			title: '测试1',
				// 			ignore: true
				// 		},
				// 		{
				// 			title: '测试2',
				// 			ignore: true
				// 		},
				// 		{
				// 			title: '测试3',
				// 			ignore: true
				// 		}
				// 	],
				// 	variableName: 'pic'
				// }
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
	onLoad() {
		uni.request({
			url: uni.getStorageSync('tempUrl') + 'wechat/getOpenId',
			data: {
				code: window.location.search.substr(6, 32)
			},
			method: 'GET',
			success: res => {
				console.log(res.data);
				uni.setStorageSync('openId', res.data);
				uni.request({
					url: uni.getStorageSync('tempUrl') + 'owner/login',
					data: {
						wechatOpenid: uni.getStorageSync('openId')
					},
					method: 'GET',
					success: res => {
						// console.log(res.data);
						if (res.data.path !== null) {
							alert('您还未绑定，请先绑定');
							uni.navigateTo({
								url: '../personaCenter/ownerByOpenid'
							});
						} else {
							// alert('已注册');
							console.log(res.data);
							// alert(res.data.data.accessToken);
							uni.setStorageSync('owner', res.data.data);
						}
					}
				});
			}
		});
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
			console.log(JSON.stringify(res));
			uni.request({
				url: uni.getStorageSync('tempUrl') + 'public/insertRepair?token=' + uni.getStorageSync('owner').accessToken,
				data: {
					title: res.title,
					content: res.content
				},
				method: 'POST',
				success: res => {
					console.log(res);
					if (res.data.code === 0) {
						uni.showToast({ title: '申请成功', icon: 'success' });
						alert('申请成功');
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
