<template>
	<view>欢迎使用。。</view>
</template>

<script>
export default {
	onLoad() {
		// uni.setStorageSync('tempUrl', 'http://localhost:8085/');
		uni.setStorageSync('tempUrl', 'http://crwy.yuanfudashi.com:8085/');
		window.location.href = uni.getStorageSync('tempUrl') + 'wechat/authorize?type=repair';
	}
};
</script>

<style></style>
