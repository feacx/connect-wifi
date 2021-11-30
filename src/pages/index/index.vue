<template>
	<view class="content">
		<view>
			{{ JSON.stringify(wifiList) }}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				wifiList: null
			}
		},
		onLoad() {
			this.startWifi();
			// wx.scanCode({
			// 	success: (res) => {
			// 		console.log("扫码结果");
			// 		console.log(res);
			// 	},
			// 	fail: (res) => {
			// 		console.log(res);
			// 	}
			// })
		},
		methods: {
			startWifi() {
				wx.showLoading({
					title: '正在搜索可连接wifi',
				})

				wx.startWifi({
					success: (res) => {
						console.log('startWifi', res)
						wx.getWifiList({
							success: () => {
								wx.onGetWifiList(res => {
									this.wifiList = res;
									wx.hideLoading({});
									console.log('getWifiList', res)
									console.log(res)
									if(res.wifiList.length !== 0) {
										// 可以对获取的wifi列表进行数据处理或者筛选
										// 根据项目要求展示wifi列表界面
									}else{
										wx.showToast({
											title: '暂无可连接wifi,请重试',
											icon: 'none'
										})
									}
								})
							},  
							fail: (e) => {
								console.log('获取wifi列表失败', e)
								wx.hideLoading({})
							}
						})
					},
					fail: (e) => {
						console.log('wifi连接失败', e)
						wx.hideLoading({})
					},
				})
			},
			
			// 选择wifi连接
			onChangeWifi(e) {
				let that = this
				let item = e.detail.data
				wx.connectWifi({
					SSID: item.SSID,
					BSSID: item.BSSID,
					password: '',
					success: function (res) {
						console.log('connectWifi suc', res)
					},
					fail: function (e) {
						console.log('connectWifi fail',e)
						wx.showToast({
							title: '设备wifi连接失败',
							icon: 'none'
						})
					}
				})
			},
		}
	}
</script>

<style lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin: 200rpx auto 50rpx auto;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
