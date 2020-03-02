<template>
	<view class="content">
		<uni-nav-bar left-icon="back" left-text="搜索" fixed  
		 color="#fff" background-color="#000000" @clickLeft ="handleBack"></uni-nav-bar>
	    <view class="searchBar">
			<view class="searchBar-left">
				<image src="../../static/search.png" mode=""></image>
				<input type="text" placeholder="请输入关键词"  @input="handleInput" @confirm="handleSearch"/>
			</view>
			<view class="searchBar-right" @click="handleSearch">				
					搜索
			</view>	    	
	    </view>
		<!-- history search -->
		<view class="historySearch">
			<view class="title">
				<image src="../../static/clock.png" mode=""></image>
				<view class="word">
					历史搜索
				</view>
			</view>			
			<view class="content">
				<view class="content-item" 
				v-for="(item,index) in searchList" 
				:key="index"
				@tap="handleSearch(item)"
				 >	
				  {{item}}
				</view>
			</view>
		</view>	
			<!-- clearHistory -->
		  <view class="clearHistory" @click="clearHistory">
		  	 清空搜索历史
		  </view>
	</view>
</template>

<script>
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	export default {
		data() {
			return {
				searchValue:'',
				searchList:[
					'葡萄酒',
					'白葡萄酒',
					'法国拉菲',
					'长城',
					'红葡萄酒',
					'起泡酒'
				]
			}
		},
		components:{
		            uniNavBar 
		         	   
				},
		methods: {
			handleBack(){
				uni.navigateBack({
					delta:1
				})
			},
			handleInput(e){
				this.searchValue=e.detail.value;
				
			},
			handleSearch(keywords){
				if(keywords){
					uni.navigateTo({
						url:'/pages/search-to-details/search-to-details?keywords='+keywords
					})
				}else{
			
					uni.navigateTo({
						url:'/pages/search-to-details/search-to-details?keywords='+this.searchValue
					})
				}
				
			},
			clearHistory(){
				this.searchList=[];
			}
		}
	}
</script>

<style lang="scss">
 .content{
	 .searchBar{
		 background:#F1F1F1;
		 height:90upx;
		 padding:10upx 10upx;
		 display:flex;
		 flex-direction: row;
		 justify-content: center;
		 align-items: center;
		 font-size:12px;
		 .searchBar-left{
			 width:80%;
	         height:60upx;
			 border-radius: 16px;
			 background:#FFFFFF;
			 display: flex;
			 flex-direction: row;
			 justify-content:flex-start;
			 align-items: center;
			 margin-right:30upx;
			 image{
				 width:40upx;
				 height:40upx;
			 }
			 input{			 
				 color:#C0C0C0;
				
			 }
		 }
		 .searchBar-right{
			  height:60upx;
			  width:15%;
			  border-radius:4px;
			  background:#E74246;
			  color:#FFFFFF;			
			  line-height: 60upx;
			  text-align: center;
		 }
	 }
	 .historySearch{
		 padding: 50upx 15upx;
		 .title{
			 display: flex;
			 flex-direction: row;
			 justify-content: flex-start;
			 font-size: 12px;			
			 image{
				 width:30upx;
				 height:30upx;
			 }
			 .word{
				 margin-left:10upx;
				 color:#C0C0C0;
			 }
		 }
		 .content{	
			 margin-top: 30upx;
			 display:flex;
			 flex-wrap: wrap;
			 .content-item{
				 padding:10upx 20upx;
				 border-radius:2px;
				 background:#EEEEEE;
				 margin-right:20upx;
				 margin-bottom:20upx;
				 font-size:12px;
			 }
		 }
		 
		 
	 }
     .clearHistory{
		 font-size: 12px;
		 width:60%;
		 height:60upx;
		 border-radius:16px;
		 border:1.5px solid #EEEEEE;
		 display:flex;
		 flex-direction: row;
		 justify-content: center;
		 align-items: center;
		 position:fixed;
		 bottom:100upx;
		 left:20%;
		
		 
	 }	 
 }
</style>
