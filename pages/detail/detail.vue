<template>
	<view>
		
		<view class="px-3 pt-3">
			<view class="radius10 line-h mb-1 bg-white">
				<view class="flex1 p-3 font-32 font-w">农机互助宝</view>
				<view class="py-3 pl-2 b-DRf5 flex-1">
					<view>保单号：{{mainData.title}}</view>
					<view class="pt-3">保险期间：{{mainData.start_time}}-{{mainData.end_time}}</view>
				</view>
			</view>
			
			
			<view class="font-32 flex1 pt-4">
				<view>险种(保额)</view>
				<!-- <view>保额</view> -->
			</view>
			<view class="bB-f5 line-h-lg py-3" v-html="mainData.content" v-show="mainData.content">
				<!-- <view class="flex1 pt-4 color6">
					<view>死亡伤残赔偿</view>
					<view>110000元</view>
				</view> -->
			</view>
			<view class="font-32 flex1 py-4">
				<view>保费合计</view>
				<view class="colorR">{{mainData.small_title}}元</view>
			</view>
			
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-3">
			<view class="font-32 pt-4">身份信息</view>
			<view class="pb-4">
				<view class="flex1 pt-4 color6">
					<view>被保人</view>
					<view>{{mainData.name}}</view>
				</view>
				<view class="flex1 pt-4 color6">
					<view>证件类型</view>
					<view>身份证</view>
				</view>
				<view class="flex1 pt-4 color6">
					<view>证件号码</view>
					<view>{{mainData.card_no}}</view>
				</view>
				<view class="flex1 pt-4 color6">
					<view>手机号</view>
					<view>{{mainData.phone}}</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-3">
			<view class="font-32  pt-4">被保车辆信息</view>
			<view class="pb-4">
				<view class="flex1 pt-4 color6">
					<view>车牌号</view>
					<view>{{mainData.keywords}}</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-3 py-4 d-flex j-end">
			<view class="btn66 b-e1">电子保单</view>
			<view class="btn66 b-e1"
			@click="Router.navigateTo({route:{path:'/pages/clause/clause'}})">保险条款</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				searchItem: {
					menu_id: 1,
					thirdapp_id: 2
				},
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.searchItem.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res)=>{
					if(res.info.data.length>0){
						self.mainData = res.info.data[0];
						var start_time = self.$Utils.timeto(parseInt(self.mainData.start_time),'ymd');
						var end_time = self.$Utils.timeto(parseInt(self.mainData.end_time),'ymd');
						self.mainData.start_time = start_time.replace(/-/g,'.');
						self.mainData.end_time = end_time.replace(/-/g,'.');
						
						var reg=/(\d{7})\d{4}/; //正则表达式
						self.mainData.phone= self.mainData.phone.replace(reg, "$1****")
						var reg1=/(\d{4})\d{10}(\d{4})/; //正则表达式
						self.mainData.card_no= self.mainData.card_no.replace(reg1, "$1**********$2")
					};
					self.$Utils.finishFunc('getMainData');
				};
				
				self.$apis.articleGet(postData, callback);
			}
			
		}
	}
</script>

<style scoped>
page{background-image: linear-gradient(to bottom,#49A0F1,#fff 22%);}
.btn66{width: 160rpx;color: #222;margin: 0 0 0 40rpx;}
</style>
