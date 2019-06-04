<template>
	<view>
		<inputs
			:fatherMethod="fatherMethod"
			:inputsArray="inputsArray"
			activeName="提交申请"
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

		<!-- <button type="primary" @tap="openTest()" style="margin-top: 50px;">打开test页面</button> -->
	</view>
</template>

<script>
import inputs from '@/components/QuShe-inputs/inputs.vue';
var flag = true;
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
					segmentationTitle: '预约申请',
					border_top: '1px solid #f2f2f2',
					type: 'switch',
					title: '是否驾车',
					color: '#c0ebd7',
					defaultValue: false,
					scale: '.8', //比例大小
					variableName: 'switch' //自定义变量名
				},
				{
					type: 'picker-custom',
					title: '选择业主住址',
					defaultValue: [0, 0, 0], //初始数据
					onceShowDefaultValue: true, //是否显示初始数据
					itemArray: [
						{
							building_num: '1幢',
							temp: [
								{
									louhao: '1楼',
									house_num: ['101室', '102室', '103室', '104室', '105室', '106室']
								},
								{
									louhao: '2楼',
									house_num: ['201室', '202室', '203室', '204室', '205室', '206室']
								},
								{
									louhao: '3楼',
									house_num: ['301室', '302室', '303室', '304室', '305室', '306室']
								},
								{
									louhao: '4楼',
									house_num: ['401室', '402室', '403室', '404室', '405室', '406室']
								},
								{
									louhao: '5楼',
									house_num: ['501室', '502室', '503室', '504室', '505室', '506室']
								}
							]
						},
						{
							building_num: '2幢',
							temp: [
								{
									louhao: '1楼',
									house_num: ['101室', '102室', '103室', '104室', '105室', '106室']
								},
								{
									louhao: '2楼',
									house_num: ['201室', '202室', '203室', '204室', '205室', '206室']
								},
								{
									louhao: '3楼',
									house_num: ['301室', '302室', '303室', '304室', '305室', '306室']
								},
								{
									louhao: '4楼',
									house_num: ['401室', '402室', '403室', '404室', '405室', '406室']
								},
								{
									louhao: '5楼',
									house_num: ['501室', '502室', '503室', '504室', '505室', '506室']
								}
							]
						},
						{
							building_num: '3幢',
							temp: [
								{
									louhao: '1楼',
									house_num: ['101室', '102室', '103室', '104室', '105室', '106室']
								},
								{
									louhao: '2楼',
									house_num: ['201室', '202室', '203室', '204室', '205室', '206室']
								},
								{
									louhao: '3楼',
									house_num: ['301室', '302室', '303室', '304室', '305室', '306室']
								},
								{
									louhao: '4楼',
									house_num: ['401室', '402室', '403室', '404室', '405室', '406室']
								},
								{
									louhao: '5楼',
									house_num: ['501室', '502室', '503室', '504室', '505室', '506室']
								}
							]
						},
						{
							building_num: '4幢',
							temp: [
								{
									louhao: '1楼',
									house_num: ['101室', '102室', '103室', '104室', '105室', '106室']
								},
								{
									louhao: '2楼',
									house_num: ['201室', '202室', '203室', '204室', '205室', '206室']
								},
								{
									louhao: '3楼',
									house_num: ['301室', '302室', '303室', '304室', '305室', '306室']
								},
								{
									louhao: '4楼',
									house_num: ['401室', '402室', '403室', '404室', '405室', '406室']
								},
								{
									louhao: '5楼',
									house_num: ['501室', '502室', '503室', '504室', '505室', '506室']
								}
							]
						},
						{
							building_num: '5幢',
							temp: [
								{
									louhao: '1楼',
									house_num: ['101室', '102室', '103室', '104室', '105室', '106室']
								},
								{
									louhao: '2楼',
									house_num: ['201室', '202室', '203室', '204室', '205室', '206室']
								},
								{
									louhao: '3楼',
									house_num: ['301室', '302室', '303室', '304室', '305室', '306室']
								},
								{
									louhao: '4楼',
									house_num: ['401室', '402室', '403室', '404室', '405室', '406室']
								},
								{
									louhao: '5楼',
									house_num: ['501室', '502室', '503室', '504室', '505室', '506室']
								}
							]
						}
					],
					steps: {
						steps_1_value: 'building_num',
						steps_2_item: 'temp',
						steps_2_value: 'louhao',
						steps_3_item: 'house_num'
					},
					linkageNum: 3, //2 表示为2级联动
					linkage: true, //true 表示开启联动
					variableName: 'aaaaaaaaaaa' //自定义变量名
				},
				{
					type: 'picker-date',
					title: '预约开始时间',
					onceShowDefaultValue: 'true',
					variableName: 'startTime' //自定义变量名
				},
				{
					type: 'picker-date',
					title: '预约到期时间',
					variableName: 'expirationTime' //自定义变量名
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
	onLoad(option) {
		// console.log(window.location.search.substr(6,32));
		uni.request({
			url: uni.getStorageSync('tempUrl') + 'wechat/getOpenId',
			data: {
				code: window.location.search.substr(6, 32)
			},
			method: 'GET',
			success: res => {
				console.log(res.data);
				// alert(res.data);
				uni.setStorageSync('openId', res.data);
				uni.request({
					url: uni.getStorageSync('tempUrl') + 'visitor/login',
					data: {
						wechatOpenid: uni.getStorageSync('openId')
					},
					method: 'GET',
					success: res => {
						// console.log(res.data);
						if (res.data.path !== null) {
							alert('您还未注册，请先注册');
							uni.reLaunch({
								url: './register'
							});
						} else {
							// alert('已注册');
							console.log(res.data);
							// alert(res.data.data.accessToken);
							uni.setStorageSync('visitor', res.data.data);
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
			console.log(res);
			const requestResult = res;

			uni.request({
				url: uni.getStorageSync('tempUrl') + 'owner/selectByHouseNum?token=' + uni.getStorageSync('visitor').accessToken,
				data: {
					buildingNum: requestResult.aaaaaaaaaaa.value[0] + 1,
					houseNum: requestResult.aaaaaaaaaaa.value[1] + 1 + '0' + (requestResult.aaaaaaaaaaa.value[2] + 1)
				},
				method: 'GET',
				success: res => {
					console.log(res);
					const responseResult = res.data;

					if (responseResult.code === 0) {
						uni.request({
							url: uni.getStorageSync('tempUrl') + 'visitorApply/insertSelective?token=' + uni.getStorageSync('visitor').accessToken,
							data: {
								ownerId: responseResult.data.id,
								visitorId: uni.getStorageSync('visitor').id,
								startTime: requestResult.startTime,
								expirationTime: requestResult.expirationTime,
								carNum: requestResult.carNum
							},
							method: 'POST',
							success: res => {
								console.log(res);
								if (res.data.code === 0) {
									alert('成功啦');
								} else {
									alert('失败');
								}
							}
						});
					} else if (responseResult.code === 2) {
						alert(responseResult.exception);
					} else {
						// alert(requestResult.aaaaaaaaaaa.value[0] + 1);
						// alert(requestResult.aaaaaaaaaaa.value[1] + 1 + '0' + (requestResult.aaaaaaaaaaa.value[2] + 1));
						alert('异常错误');
					}
				}
			});
		},
		fatherMethod() {
			// 动态改变inputsArray示例 （inputs中watch监听的是inputsArray的内存地址，所以要改变inputsArray要改变他的内存地址，要重新赋值）
			if (flag) {
				this.inputsArray = this.inputsArray.concat({
					title: '车牌号码',
					type: 'input',
					variableName: 'carNum' //自定义变量名
				});
				flag = false;
			} else {
				this.inputsArray = this.inputsArray.slice(0, this.inputsArray.length - 1);
				flag = true;
			}
		}
	},
	components: {
		inputs
	}
};
</script>

<style></style>
