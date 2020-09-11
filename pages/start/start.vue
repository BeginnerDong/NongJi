<template>
	<view>
		
		<image src="../../static/images/img.png" class="p-fXY"></image>
		
		<view class="p-r">
			<image src="../../static/images/logo.png" class="logo"></image>
			
			<input type="text" v-model="keywords" placeholder="请输入保单号或身份证号" />
			<view class="btn80 Mgb" @click="search">搜索</view>
		</view>
		
		<!-- @click="Router.navigateTo({route:{path:'/pages/index/index'}})" -->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				keywords:'',
				mainData:[]
			}
		},
		methods: {
			
			search(){
				const self = this;
				if(!self.keywords){
					self.$Utils.showToast('请输入查询条件', 'none')
				}else{
					self.Router.navigateTo({route:{path:'/pages/index/index?keywords='+self.keywords}});
				}
			}
			
			
			
			
			
		}
	}
</script>

<style scoped>
page{height: 100%;overflow: hidden;}
.logo{width: 181rpx;height: 206rpx;margin: 90rpx auto 0;}
input{width: 560rpx;height: 80rpx;background-color: #fff;margin: 40% auto 50rpx;font-size: 26rpx;border-radius: 10rpx;box-sizing: border-box;text-align: center;}
.btn80{width: 560rpx;}
</style>
