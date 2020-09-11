<template>
	<view>
		
		<view class="font-34 text-c line-h pt-4 font-w pb-2">{{mainData.title}}</view>
		<view class="font-24 pb-4 line-h text-c font-w">【{{mainData.description}}】</view>
		
		<view class="px-3 line-h-lg" v-html="mainData.content">
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
				searchItem: {
					menu_id: 2,
					thirdapp_id: 2,
					id:1
				},
			}
		},
		onLoad(options) {
			const self = this;
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
					};
					self.$Utils.finishFunc('getMainData');
				};
				
				self.$apis.articleGet(postData, callback);
			}
			
		}
	}
</script>

<style>

</style>
