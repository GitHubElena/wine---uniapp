<template>
	<view class="content">
		<uni-nav-bar left-icon="back" left-text="订单确认" fixed
		color="#fff" background-color="#000000" 
		@clickLeft ="handleBackIndex">
		</uni-nav-bar>
		<view class="goodsInfo">
			 <view class="info-left">
			 	 <image :src="pic" mode=""></image>
			 </view>
			 <view class="info-right">
			 	   <view class="descTitle">
			 	   	  {{descTitle}}
			 	   </view>
				   <view class="descEnglish">
				   	  {{descEnglish}}
				   </view>
				   <view class="desc-bottom">
				   	   <view class="bottom-left">
				   	   	  {{descPrice}}
				   	   </view>
					  <view class="bottom-right">
						  x {{chooseNumber}}
					  </view>
				   </view>
			 </view>
		</view>
		<view class="totalCount">
			<view class="totalCount-left">
				跨境
			</view>
			<view class="totalCount-right">
				 <view class="right-item-word">
				 	共{{chooseNumber}}件商品 小计: 
				 </view>
				 <view class="right-item-money">
				 	 {{descPrice}}
				 </view>
			</view>
		    
		</view>
	    
		 <view class="bottom-content">
	     	  <view class="public right">
				 <view class="fee">运费: </view> 
				 <view class="active">+ ￥0</view>	
	     	  </view>
			  <view class="public left">
			  	 会员折扣
			  </view>
			  <view class="public between">
			  	 <view class="score-left">
			  	 	积分兑换
			  	 </view>
				 <view class="score-right">
				 	<image src="../../static/right.png" mode="" class="right-png"></image>
				 </view>
			  </view>
			  <view class="public between">
			  	 <view class="score-left">
			  	 	购买凭证
			  	 </view>
				 <view class="public2 between">
					 <view class="btw-left">
					 	跨境购物仅支持
						<text class="active">开形式发票</text>
					 </view>
					<image src="../../static/right.png" mode="" class="right-png"></image>
				 </view>
			  </view>
			  
			  <view class="public between">
			  	 <view class="score-left">
			  	 	消费者须知
			  	 </view>
				 <view class=" public between">
					 <view class="btw-left">
						 购买即视知晓
						<text class="active">消费者须知</text>
					 </view>
					<image src="../../static/right.png" mode="" class="right-png"></image>
				 </view>
               </view>
			   
			   <view class="column">
			   	   <view class="left">
			   	   	  配送信息
			   	   </view>
				   <view class=" public between">
				   	  <view class="btw-left">
				   	  	 新增地址
				   	  </view>
					  <image src="../../static/right.png" mode="" class="right-png"></image>
				   </view>
			   </view>
			   
			   <view class="public between">
			   	  <view class="btn-left">
			   	  	买家留言
			   	  </view>
				  <view class="btn-right">
				  	 选填: 对本次交易的说明
				  </view>
			   </view>
			   <view class="public2 left">
			   	   本次消费将获得 <text class="active">2558</text>积分
			   </view>		
	     </view>
		
	    <view class="btn-fix">
	    	 <view class="btn-gray">
	    	 	 实付金额:    {{descPrice}}
	    	 </view>
			 <view class="btn-red">
			 	 订单结算
			 </view>
	    </view>
	</view>
</template>

<script>
 import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	export default {
		data() {
			return {
				goodsId:1,
				chooseNumber:1,
				descTitle:'',
				descEnglish:'',
				pic:'',
				descPrice:0
			}
		},
		onLoad(options) {
			let {goodsId,chooseNumber,descTitle,descEnglish,descPrice,pic}=options;
			this.goodsId=goodsId;
			this.chooseNumber=chooseNumber;
			this.descTitle=descTitle;
			this.descEnglish=descEnglish;
			this.descPrice=descPrice;
			this.pic=pic;
			
		},
		components:{
		            uniNavBar 		         	   
		},
		methods: {
			handleBackIndex(){
				uni.navigateBack({
					delta:1
				})
			}
		}
	}
</script>

<style lang="scss">
  .content{
	  .goodsInfo{
		  border:1px solid #EEEEEE;
		  display: flex;
		  flex-direction: row;
		  .info-left{
			  margin:15upx 0px;
			  width:35%;
			  image{
				  width:100%;
				  height:230upx;
				  
			  }
		  }
		  .info-right{
			  margin:15upx 0px;
			  padding:20upx ;
			  font-size:12px;
			  width:75%;
			  display: flex;
			  flex-direction: column;
			  border-left:1px solid #EEEEEE;
			 .desc-bottom{
				 margin-top:80upx;
				 display: flex;
				 flex-direction: row;
				 justify-content: space-between;
			 } 
		  }
	  }
      .totalCount{		  
		  padding:20upx;
		  font-size: 14px;
		  border-bottom:20upx solid #EEEEEE;
		  display: flex;
		  flex-direction: row;
		  justify-content: space-between;
		  .totalCount-left{
			  width:16%;
			  height:60upx;
			  text-align: center;
			  line-height: 60upx;
			  border:1px solid black;
		  }
		  .totalCount-right{		 
			   width:50%;
			   display: flex;
			   flex-direction: row;
			   justify-content: space-between;
			   .right-item-word{
				   
			   }
			   .right-item-money{
				   margin-left:15upx;
				   color:#E74246;
			   }
		  }
	  }
      .bottom-content{
		   padding:20upx;
		  font-size:12px;
		  margin-bottom:150upx;
		  .active{
			  color:#E74246;
		  }
		  .btn-right{
			  color:#C8C7CC;
			}
	      .left{
			   justify-content: flex-start;
		   }
		   .right{
		   	    justify-content: flex-end;
		        height:60rpx
		   }
		   .column{
			   flex-direction: column;
			   height:120upx;
			   display: flex;
		   }
		   .right-png{
			   width:40upx;
			   height:40upx;
		   }
		   .between{
			   justify-content: space-between;
		   }
		  .public{
			  display: flex;			  
		      flex-direction: row;			 
			  border-bottom:1px solid #C8C7CC;
			  height:80upx;
			  align-items: center;
		  }
		  .public2{
		  	  display: flex;			  
		      flex-direction: row;			 	  		
			  height:80upx;
			  align-items: center;
		  }
		 
	  }
      .btn-fix{
		  position:fixed;
		  z-index:99;
		  bottom:0;
		  left:0;
		  right:0;
		  height:100upx;
		  display: flex;
		  justify-content: space-between;
		  flex-direction: row;
		  color:#FFFFFF;
		  font-size:12px;
		  .btn-gray{
			  padding-left:30upx;
			  background: #6C6C6C;
			  line-height: 100upx;
			  width:75%;
		  }
		  .btn-red{
			  background:#E74246;
			  line-height: 100upx;
			  width:25%;
			  text-align: center;
		  }
	  }
  }
</style>
