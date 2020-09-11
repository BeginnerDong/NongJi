<template>
	<view>
		
		<view class="b-e1 radius10 line-h mx-3 mt-3" v-for="(item,index) in mainData" :key="index">
			<view class="flex1 p-3">
				<view class="font-30">农机互助宝</view>
				<view class="flex color9 font-24"
				@click="goDetail(item)">
					<text>查看详情</text>
					<image src="../../static/images/icon4.png" class="jt-icon ml-1"></image>
				</view>
			</view>
			<view class="flex b-DTf5">
				<view class="font-26 py-3 pl-2 b-DRf5 flex-1">
					<view class="flex">
						<image src="../../static/images/icon1.png" class="wh32 mr-1"></image>
						<view><text class="color8">互保车辆：</text>{{item.keywords}}</view>
					</view>
					<view class="flex pt-3">
						<image src="../../static/images/icon.png" class="wh32 mr-1"></image>
						<view><text class="color8">保险期间：</text>{{item.start_time}}-{{item.end_time}}</view>
					</view>
				</view>
				<view class="flex4 px-4">
					<image src="../../static/images/icon2.png" class="bz-icon" v-if="item.time==1"></image>
					<image src="../../static/images/icon3.png" class="tz-icon" v-else></image>
					<view class="font-24 pt-2" :class="item.time==1?'colorM':'colorR'">{{item.time==1?'保障中':'已过期'}}</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				paginate:{},
				mainData:[],
				keywords:''
			}
		},
		onLoad(options) {
			const self = this;
			self.keywords = options.keywords;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		methods: {
			
			goDetail(item){
				const self = this;
				if(item.time==1){
					self.Router.navigateTo({route:{path:'/pages/detail/detail?id='+item.id}})
				}else{
					self.$Utils.showToast('已过期，不可查看', 'none')
				}
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.keywords = self.keywords
				const callback = (res)=>{
					if(res.info.data.length>0){
						self.mainData = res.info.data;
						for(var i=0; i<self.mainData.length; i++){
							var time = new Date().getTime();
							if(self.mainData[i].end_time>time){
								self.mainData[i].time = 1;
							}else{
								self.mainData[i].time = 0;
							}
							
							var start_time = self.$Utils.timeto(parseInt(self.mainData[i].start_time),'ymd')
							var end_time = self.$Utils.timeto(parseInt(self.mainData[i].end_time),'ymd')
							self.mainData[i].start_time = start_time.replace(/-/g,'.');
							self.mainData[i].end_time = end_time.replace(/-/g,'.');
						}
					};
					self.$Utils.finishFunc('getMainData');
				};
				
				self.$apis.searchOrder(postData, callback);
			}
			
		}
	};
</script>

<style scoped>
.jt-icon{width: 10rpx;height: 18rpx;}
.bz-icon{width: 42rpx;height: 48rpx;}
.tz-icon{width: 40rpx;height: 42rpx;}
</style>