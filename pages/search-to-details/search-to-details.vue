<template>
	<view class="content">
         <uni-nav-bar left-icon="back" left-text="筛选结果" fixed  
		 color="#fff" background-color="#000000" 
		 @clickLeft ="handleBackIndex">
		 </uni-nav-bar>
		 <view class="wine-content">
		 	<view class="general-title">
		 		 <view class="general-title-left">
		 		 	有240瓶葡萄酒
		 		 </view>
				 <view class="general-title-right" @tap="handleBackIndex">
				 	重新搜索
				 </view>
		 	</view>
			<view class="general-content">
				<view class="content-item" v-for="(item,index) in wineList" :key="item.id" @tap="navigateGoodsDetails(item.id)"> 					 
					 <view class="item-word" >
						 <image :src="item.url" mode=""></image>
						  <view class="item-word-chinese">
							  {{item.chinese.substr(0,10)+'...'}}
						  </view>
						  <view class="item-word-english">
							   <view class="english-left">
								   {{item.english.substr(0,10)+'...'}}
							   </view> 
							   <view class="english-right">
								   干型
							   </view>
						  </view>
						  <view class="item-word-price">
							   {{item.price}}
						  </view>
					    </view>
					  <view class="item-bottom">			  	
					  </view>
				</view>
				
			</view>
		 </view>
	</view>
</template>

<script>
    import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	export default {
		data() {
			return {
				wineList:[
					
				  { 
					id:1,
					url:'../../static/search-wine10.png',
					chinese:'新西兰 马尔堡 首彩苏维瓮白...',
					english:'sarahs creek pr...',
					price:'￥588'
				  },
				  
				  {
					id:2,
					url:'../../static/search-wine4.png',
					chinese:'法国 波尔多 罗斯拆尔德男爵...',
					english:'sarahs creek pr...',
					price:'￥199'
				  },
				  {
					id:3,
					url:'../../static/search-wine9.png',
					chinese:'法国 波尔多 罗斯拆尔德男爵...',
					english:'sarahs creek pr...',
					price:'￥499'
				  },
				  {
					id:4,
					url:'../../static/search-wine8.png',
					chinese:'法国 波尔多 罗斯拆尔德男爵...',
					english:'sarahs creek pr...',
					price:'￥599'
				  },
				  {
					id:5,
					url:'../../static/search-wine7.png',
					chinese:'法国 波尔多 罗斯拆尔德男爵...',
					english:'sarahs creek pr...',
					price:'￥179'
				  },
				  {
					id:6,
					url:'../../static/search-wine2.png',
					chinese:'法国 波尔多 罗斯拆尔德男爵...',
					english:'sarahs creek pr...',
					price:'￥299'
				  },
				  {
					id:7,
					url:'../../static/search-wine7.png',
					chinese:'法国 波尔多 罗斯拆尔德男爵...',
					english:'sarahs creek pr...',
					price:'￥159'
				  },
				]
			}
		},
		onLoad(keywords) {
			//console.log(keywords)
		},
		components:{
		            uniNavBar 		         	   
		},		
		methods: {
			handleBackIndex(){
				uni.redirectTo({
					url:'/pages/index/index'
				})
			},
			navigateGoodsDetails(id){
				uni.redirectTo({
					url:'/pages/wine-details/wine-details?goodsId='+id
				})
			}
		}
	}
</script>

<style lang="scss">
   .content{
	   .wine-content{
		   .general-title{
			   font-size:12px;			 
			   display: flex;
			   flex-direction: row;
			   .general-title-left{
				   width:50%;
				   height:60upx;				  
					text-align:center ;
					line-height: 60upx;;
			   }
			   .general-title-right{
				   width:50%;
			       height:60upx;
				   text-align:center ;
				   line-height: 60upx;;
			   }
		   }
		   .general-content{
			   
			   padding:10upx;
			   display: flex;
			   flex-wrap: wrap;
			   font-size: 12px;
			   .content-item:nth-child(odd)>.item-word{
				   border-right:1px solid #EEEEEE;
			   }
			   .content-item{ 				   
				   width:48%;		
				   image{
					   width:100%;
					   height:400upx;				  
					  
				   }	
				   .item-word{
					 
				       padding:5upx 25upx;
				     .item-word-english{ 				   
					    display: flex;
					    flex-direction: row;
					    justify-content: space-between;
							 .english-right{
								 font-size:8px;
								 background:purple;
								 border-radius:16px;
								 padding:5upx 10upx;
								 color:#FFFFFF;
								
							 }
				       }
					   .item-word-price{
						   color:#E74246;
						   font-weight:bold;
					   }
				   }
			        .item-bottom{
						width:100%;
						height:1px;
						background:#EEEEEE;
						display: block;
						margin:20upx 0upx;
					}
			   }
		      
			  
		   }
	   }
   }
</style>
