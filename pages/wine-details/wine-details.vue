<template>
	<view class="content">
		<uni-nav-bar left-icon="back" left-text="商品详情" fixed
		color="#fff" background-color="#000000" 
		@clickLeft ="handleBackIndex">
		</uni-nav-bar>
		<!-- 轮播图 -->
		  <swiper class="swiper-item" circular="true" @change="swiperChange">
				<swiper-item v-for="(item,index) in banner" :key="index">
					<view class="image-wrapper">
						<image :src="item.src">
						</image>
					</view>
				</swiper-item>				
		  </swiper>
		  <view class="right-top">
		  	 <view class="zan" v-if="zan===false">
		  	 	<image src="../../static/heart-gray.png" mode="" @tap="handleChange(0)"></image>
		  	 </view>
			 <view class="zan" v-else>
			 	<image src="../../static/heart.png" mode="" @tap="handleChange(1)"></image>
			 </view>
			 <view class="share" >
				 <button @click="shareToFriends"  open-type="share">	 	       
				</button>
				 <image src="../../static/share.png" mode="" ></image>
			 </view>
		  </view>
		  <view class="indicator">{{currentSwiper+1}}/{{banner.length}}</view>
		  <view class="desc">
		  	 <view class="desc-title" v-show="descTitle">
		  	 	 {{descTitle}}
		  	 </view>
			 <view class="desc-volumn"  v-show="descVolumn">
			 	{{descVolumn}}
			 </view>
			 <view class="desc-content"  v-show="descContent">
			 	{{descContent}}
			 </view>
			 <view class="desc-english"  v-show="descEnglish">
			 	{{descEnglish}}
			 </view>
			 <view class="desc-price"  v-show="descPrice">
			 	{{descPrice}}
			 </view>
		  </view>
	      <view class="bg-gray">
	       	 <view class="appreciate">
	       	 	 <view class="appreciate-left">
	       	 	 	<image src="../../static/appreciate.png" mode=""></image>
	       	 	 </view>
				 <view class="appreciate-right">
				 	<view class="appreciate-count">
				 		{{count}}
				 	</view>
					<view class="appreciate-zan">
						<image src="../../static/zan.png" mode=""></image>
					</view>
				 </view>
	       	 </view>
			 <!-- 商品详情及评价 -->
	         <view class="tabbar">
	         	  <view class="tab-title"
				   v-for="(item,index) in tabTitle" 
				   :key="index"
				   :class="[index==showCategoryIndex?'on':'']"
				   @tap="showCategory(index)"
					>
	         	  	  {{item}}
	         	  </view>
	         </view>
	
		  </view>
	      <!-- 底部内容 -->
	      <view class="bottom-content">
			  <view class="" v-if="showCategoryIndex===0">
				<goodsDetails/>	
			  </view>
			  <view class="" v-else>
			  	  <goodsEvaluation/>
			  </view>
	      </view>
		  
		  <view class="bottom-fixed">
				<view class="kefu" @click="toPageKefu">
					 <view class="kefu-icon">
						 <image src="../../static/kefu.png" mode=""></image>
					 </view>
					 <view class="kefu-word" >
						客服
					 </view>
				</view>
				<view class="shop-cart">
					<image src="../../static/cart.png" mode=""></image>
					<view class="shop-count">
						{{shopCount}}
					</view>
					<view class="cart-word">
						购物车
					</view>
				</view>
				<view class="add-cart" @tap="toPop(1)">
					加入购物车
				</view>
				<view class="bug" @tap="toPop(2)">
					立即购买
				</view>
		  </view>
		  <uni-popup ref="popup" type="bottom">
			  <view class="popup">
				   <image src="../../static/close.png" mode="" class="close" @click="closeCart"></image>
		  	        <view class="popup-top">
						<image src="../../static/reduce.png" mode="" @click="reduceCount"></image>
		  	        	 <view class="number">
		  	        	     {{chooseNumber}}
		  	        	 </view>
						 <image src="../../static/add.png" mode="" @click="addCount"></image>
						 <view class="store">
						 	库存: {{storeCount}}
						 </view>
		  	        </view>
					<view class="popup-bottom" @click="numToCart">
						{{popupBottomWord}}
					</view>
		        </view>
		  </uni-popup>
	</view>
</template>

<script>
 import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
 import goodsDetails from "@/components/goods-details.vue"
 import goodsEvaluation from "@/components/goods-evaluation.vue"
 import uniPopup from "@/components/uni-popup/uni-popup.vue"
	export default {
		data() {
			return {
				   goodsId:1,
				   popupBottomWord:'加入购物车',
				   chooseNumber:1,
				   storeCount:12,
				   shopCount:0,
				   zan:false,
				   showCategoryIndex:0,
				   currentSwiper: 0,
				   banner:[
					 {
						src:'../../static/details-banner1.png'
					 },
					 {
						 src:'../../static/details-banner2.png'
					 },
					 {
						 src:'../../static/details-banner3.png'
					 }	
					],
					descTitle:'新西兰 马尔堡 首彩苏维瓮白',
					descContent:'采收.酿制和装瓶得到了与其正牌酒同样...',
					descEnglish:'sarahs creek pr...',
					descPrice:'￥588',
					descVolumn:'750ml*5',
					count:0,
					tabTitle:['商品详情','商品评价']
					
				}										
			}						
		,
		components:{
		            uniNavBar,
					goodsDetails,
					goodsEvaluation,
					uniPopup
					
					
		},	
		onLoad(goodsId) {
			
			console.log(goodsId)
		},
		onShareAppMessage(res) {
		      return {
		        title: '微信小程序测试分享',
		        path: '/pages/wine-details/wine-details'
		      }
		  },
		methods: {
			  toPageKefu(){
				  uni.navigateTo({
				  	url:'/pages/kefu/kefu'
				  })
			  },
			  handleBackIndex(){
					uni.navigateBack({
						delta:1
					})
		      },
			  swiperChange(e){
				 this.currentSwiper = e.detail.current;
			  },
			  showCategory(index){			 
				  this.showCategoryIndex=index;
			  },
			  handleChange(type){
				  this.zan=!this.zan;
				  if(type===0){
					  uni.showToast({
					  	title:'收藏成功',
					  	duration:1000
					  })
				  }else{
					  uni.showToast({
					  	title:'取消收藏成功',
						duration:1000
					  })
				  }
				  
			  },
			  toPop(type){
				  if(type===1){
					  this.popupBottomWord='加入购物车';
					 
				  }else{
					   this.popupBottomWord='立即购买';
				  }
				   this.$refs.popup.open();
				 
			  },
			  closeCart(){
				   this.$refs.popup.close()
			  },
			  reduceCount(){
				  if(this.chooseNumber===1){
					return   this.chooseNumber=1;
				  }
				  return this.chooseNumber--
			  },
			  addCount(){
				  if(this.chooseNumber===this.storeCount){
					  return this.chooseNumber=this.storeCount;
				  }
				  return this.chooseNumber++
			  },
			  numToCart(){
				  this.shopCount=this.chooseNumber;
				  if(this.popupBottomWord==='加入购物车'){
					  uni.showToast({
					  	title:'加入购物车成功',
					  	duration:1000
					  })
				  }else{
					  this.$refs.popup.close();
					  uni.navigateTo({
					  	url:'/pages/order-sure/order-sure?goodsId='
						+this.goodsId+'&chooseNumber='
						+this.chooseNumber+'&descTitle='
						+this.descTitle+'&descEnglish='
						+this.descEnglish+'&descPrice='
						+this.descPrice+'&pic='
						+this.banner[0].src
					  })
				  }
				 
			  }
			
	   }
}
</script>

<style lang="scss">
 .content{
	 .swiper-item{
		 display: flex;
		 justify-content: center;
		 align-content: center;
		 height: 650upx;
		 overflow: hidden;
		 .image-wrapper{
		 	width: 100%;
		 	height: 100%;
			image {
				width: 100%;
				height: 100%;
			}
		 }
		 
	 }
     .indicator{
			display: flex;
			justify-content: center;
			align-items: center;
			padding: 0 25upx;
			height: 40upx;
			border-radius: 40upx;
			font-size: 22upx;
			position: absolute;
			top: 750upx;
			right: 20upx;
			color: #fff;
			background-color: rgba(0, 0, 0, 0.2);
     }
	 .right-top{
		 display: flex;
		 flex-direction: row;
		 justify-content: space-between;
		 height:40upx;
		 position: absolute;
		 top:200upx;
		 right:20upx;
		 z-index:1000;
		 .zan{
			 margin-right:20upx;
		 }
			 image{
				 width:40upx;
				 height:40upx;
			 
		 }
	 }
	 .desc{
		 display: flex;
		 flex-direction: column;
		 justify-content: center;
		 align-items:center;
		 .desc-price{
			 margin-top:20upx;
			 padding-bottom:30upx;
			 color:#E74246;		
		 }
	 }
     .bg-gray{
		 padding-top:20upx;
		 background: #EEEEEE;		 
		 .appreciate{
			 width:100%;			
			 background: #FFFFFF;
			 display: flex;
			 flex-direction: row;
			 justify-content: space-between;
			 .appreciate-left{
				 padding-left:10upx;
				 width:60%;
				 image{
					 height:130upx;
					 width:100%;
				 }
			 }
			 .appreciate-right{
				 padding-right:10upx;
				 width: 10%;
				 display: flex;
				 flex-direction: row;
				 justify-content: space-between;
				 align-items: center;
				 .appreciate-count{
					 color:#C8C7CC;
					 font-size:12px;
					 
				 }
				 .appreciate-zan{
					 margin-left:10upx;
					 image{
						 width:40upx;
						 height:70upx;
						 }
					
				 }
			 }
		 }
		 .tabbar{
			 margin-top:20upx;
			 display: flex;
			 // flex-direction: row;
			 .tab-title{
				 width:50%;
				 height:80upx;
				 display: flex;
				 border:.5px solid #EEEEEE;
				 background:#F6F6F6;
				 justify-content: center;
				 align-items: center;
				 &.on{
					 background:#C8C7CC;
				 }
			 }
		 }
	 }
     .bottom-content{
		 margin-top:20upx;
		 width:100%;
	 }
	 .bottom-fixed{
		 z-index:97;
		 background: #FFFFFF;
		 position: fixed;
		 bottom:0upx;
		 left:0;
		 right:0;
		 display: flex;
		 flex-direction: row;
		 justify-content: space-between;
		 height:100upx;
		 .kefu{
			 width: 20%;
			 display: flex;
			 flex-direction: column;
			 justify-content: center;
			 align-items: center;
			 .kefu-icon{
				 image{
					 width:40upx;
					 height:40upx;
				 }
			 }
			 .kefu-word{
				 font-size:12px;
			 }
			 
		 }
		 .shop-cart{
			 display: flex;
			 flex-direction: column;
			 justify-content: center;
			 align-items: center;
			 width:20%;
			 border-left:1px solid #C8C7CC;
			 image{
				 width:40upx;
				 height:40upx;
			 }
			 .shop-count{
				  z-index:98;
				 background: #E74246;
				 border-radius:50%;
				 width: 30upx;
				 height:30upx;
				 position: fixed;
				 bottom:68upx;
				 left:199upx;
				 color:#FFFFFF;
				 font-size:10px;
				 text-align: center;
				 
				 
			 }
			 .cart-word{
				  font-size:12px;
			 }
		 }
		 .add-cart,.bug{
			 text-align: center;
			 line-height: 100upx;
			 width:40%;
			 font-size:14px;
			 background:#E74246;
			 color:#FFFFFF;
		 }
		 .bug{
			 text-align: center;
			 line-height: 100upx;
			 width:40%;
             font-size:14px;
			 background:#DD524D;
			 color:#FFFFFF;
		 }
	 }
      .popup{     
		  width:90%;
		  height:400upx;
		  background:#FFFFFF;
		  border-radius:8px;
		  margin:0 auto;
		  margin-bottom:30upx;
		  display: flex;
		  flex-direction: column;
		  position: relative;
		  .close{
			  width:40upx;
			  height:40upx;
			  position: absolute;
			  top:30upx;
			  right:10upx;
			  
		  }
		  .popup-top{
			  display: flex;
			  margin-top:120upx;
			  margin-bottom:50upx;
			  align-items: center;
			  flex-direction: row;
			  justify-content: center;
			  .number{
				  font-size: 14px;
				  margin:0 20upx;
				 
			  }
			  .store{
				  font-size: 12px;
				  color:#C8C7CC;
				  margin-left: 20upx;
			  }
			  image{
				  width:100upx;
				  height:100upx;
				  
			  }
			  
		  }
		  .popup-bottom{
			 
			  width:80%;
			  background:#E74246;
			  border-radius: 4px;
			  height:80upx;
			  margin: 0 auto;
			  text-align: center;
			  line-height: 80upx;
			  color:#FFFFFF;
			  font-size: 14px;
		  }
	  }
 }
</style>
