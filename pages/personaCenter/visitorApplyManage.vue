<template>
	<view>
		<uni-list v-for="index in content" :key="index.id">
			<uni-list-item
				:thumb="index.photo"
				:title="index.name"
				:note="'时间段：' + index.startTime + ' 至 ' + index.expirationTime"
				show-switch="true"
				show-arrow="false"
				:switch-checked="index.status === 1"
				@switchChange="changeStatus($event, index)"
			></uni-list-item>
		</uni-list>
		<view v-show="content.length === 0">暂无访客申请拜访哦</view>
	</view>
</template>

<script>
import uniList from '@/components/uni-list/uni-list.vue';
import uniListItem from '@/components/uni-list-item/uni-list-item.vue';
export default {
	components: { uniList, uniListItem },
	data() {
		return {
			content: []
		};
	},
	onLoad() {
		const that = this;
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
								url: './ownerByOpenid'
							});
						} else {
							// alert('已注册');
							console.log(res.data);
							// alert(res.data.data.accessToken);
							uni.setStorageSync('owner', res.data.data);
							uni.request({
								url: uni.getStorageSync('tempUrl') + 'visitorApply/selectListByOwnerId',
								data: {
									token: uni.getStorageSync('owner').accessToken,
									ownerId: uni.getStorageSync('owner').id,
									pageNum: 1,
									pageSize: 99
								},
								method: 'GET',
								success: res => {
									if (res.data.code === 0) {
										that.content = res.data.content;
									} else {
										alert('请求过快，请刷新后尝试');
									}
								}
							});
						}
					}
				});
			}
		});
	},
	methods: {
		changeStatus(option, index) {
			uni.request({
				url: uni.getStorageSync('tempUrl') + 'visitorApply/updateByStatus?token=' + uni.getStorageSync('owner').accessToken,
				data: {
					id: index.id,
					status: option.value ? '1' : '0'
				},
				method: 'PUT',
				success: res => {
					console.log(res);
					if (res.data.code === 0) {
						alert('状态已修改');
						console.log(res.data);
					} else {
						alert('状态修改失败');
					}
				}
			});
		}
	}
};
</script>

<style></style>
