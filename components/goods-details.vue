<template>
	<view class="content">
		<view class="list" v-for="(item ,index) in detailsList" :key="index">
			<image :src="item" mode=""></image>
		</view>
		<view class="bottom">
			----- 我是有底线的-----
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				detailsList:[
					'../static/details-img1.png',
					'../static/details-banner2.png',
				    '../static/details-img3.png',
					'../static/details-img4.png',
				
					
				]
			};
		}
	}
</script>

<style lang="scss">
  .content{
	  .list{
		  image{
			  width:100%;
			  height:900upx;
			  margin-bottom:70upx;
			
		  }
	  }
	  .bottom{
		  margin-top:40upx;
		  color:#C8C7CC;
		  font-size: 12px;
		  display: flex;
		  height:40upx;
		  justify-content: center;
		  align-items: center;
	  }
  }
</style>
