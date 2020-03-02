<template>
	  <view class="content">
		  <!-- #ifdef MP-WEIXIN -->
			<uni-nav-bar  left-text="酒加 | 首页" fixed  status-bar  color="#fff" background-color="#000000">			
			</uni-nav-bar>	  
		 <!-- #endif -->
		 <!-- #ifdef H5 || APP-PLUS-->
		        <uni-nav-bar  title="酒加 | 首页" fixed  color="#fff" background-color="#000000">			
		       </uni-nav-bar>	  
		 <!-- #endif -->
		 <!-- search -->
		 <view class="nav-search">
		 	 <view class="position">
				   <view class="position-start">
				   	  <image src="/static/position.png" mode="" class="position-img"></image>	 
				   </view>
		 	 	   <view class="position-end">
		 	 	   	   <view class="position-content" @click="checkMap">{{detailAddress}}</view>		
		 	 	   </view>
				  		
		 	 </view>
			 <view class="search" @click="searchWine">
				  <view class="search-start">
						寻找世界美酒 
				  </view> 
				  <view class="search-end">
						<image src="../../static/search.png" mode="" class="search-img"></image>
				  </view>
			 </view>
			 <view class="scan-code" @click="scanCode">
			     	<image src="../../static/scan-code.png" mode="" class="scan-code-img"></image>
			 </view>
			  
		 </view>
		 <view class="tab-select">
		 	   <tabControl :current="current" :values="navList" bgc="black" 
			               activeColor="white"  :scrollFlag='true' :isEqually='false'
							@clickItem="onClickItem" >
			    </tabControl>
		 	  
		 </view>
		
	  </view>
</template>

<script>
	 import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	 import tabControl from '@/components/tabControl-tag/tabControl-tag.vue';
	 import QQMapWX  from '@/libs/qqmap-wx-jssdk.min.js'
	export default {		
		data() {
			return {			
				 show:false,
				 addressName: '',
			  	 location:{
					 lng:'',
					 lat:''
				 },
			     current:0,
			     navList: ['推荐', '新品', '限量购', '价格', '名庄','产地'],
				
			}
		},
		onLoad(options) {
			//second touch events,the address can contain by redirect url params
	      	if(options.address){			
				this.addressName=options.address;
				this.location.lng=options.longitude;
				this.location.lat=options.latitude;
				
			}else{	
			//first touch events ,the address need reverseGeocoder Chinese by latitude and longitude.
			const qqmapsdk= new QQMapWX({ key: 'LXCBZ-NNIKD-UZ64F-H6AFI-UNJLH-OCFGE'});           
			uni.getLocation({
              type: 'gcj02',
              success:(res)=>{	
				 qqmapsdk.reverseGeocoder({
					 location:{
						 latitude:res.latitude,
						 longitude:res.longitude
					 },
					 get_poi:0,
					 success:(r)=>{
						const {address,ad_info}=r.result;
						const {location}=ad_info;
						this.addressName=address;
						this.location=location;										 	 
					 }
				 })
                }
			 });
			}
			 
		},
	    components:{
		            uniNavBar ,
		           tabControl 		   
				},
		computed:{
			detailAddress:function () {
				return this.addressName.substr(0,6)+'...';
			}
		},
		methods: {
			
              checkMap(){
				  const {lat,lng}=this.location;
				 uni.navigateTo({
				     url: '/pages/location/location?latitude='+lat+'&longitude='+lng
				 })
			  },			 
			 searchWine(){
				 uni.navigateTo({
				 	url:'/pages/wine-search/wine-search'
				 })
			 },
			 scanCode(){
				 uni.scanCode({
					success:(res)=>{	
						  uni.showModal({
						  	title:'酒加.Wine+',
							content:'扫描结果:'+res.result,
							cancelText:'取消',
							confirmText:'确定'
						  })						
					}
				 })
			 },
			  onClickItem(val) {
			             this.current = val.currentIndex
			   },
			   scollSwiper(e){
			             this.current = e.target.current
			   },
			setData(obj) {
				Object.assign(this, obj)
			}
			
			  
		}
	}
</script>

<style lang="scss" scoped>
     .nav-search{
		  padding-right: 10upx;
		  background-color:#000000;
		  color:#FFFFFF;
		  display: flex;
		  flex-direction: row ;
		  justify-content: space-between;
		  .position{
			  display: flex;
			  flex-direction: row ;
			  justify-content: space-between;  
			  font-size:12px;		 
			  .position-img{
			  			  width:50upx;
			  			  height:50upx;
			  			 
			  }
			  .position-content{
				   height:50upx;
				   line-height:50upx;		  
				   }
		  }
		  .search{ 
			  width:55%;
			  height:50upx;
			  font-size:12px;	
			  line-height: 50upx;	
			  color:#C0C0C0;		 
			  border-bottom:1px solid red;
			  display: flex;
			  flex-direction: row ;
			  justify-content: space-between;  
			  .search-end{
				  .search-img{
					  width: 50upx;
					  height:50upx;
				  }
				
			  }
		  }
          .scan-code{
			   .scan-code-img{
				   width:40upx;
				   height:50upx;
				   
			   }
		   }
	 }
	 
	
	 
	 
	 

		 
		
		 
	 
	 

</style>
