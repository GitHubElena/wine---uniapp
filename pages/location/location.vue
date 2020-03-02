<template>
	
	<view class='container'>
		<map id='map' :scale='map.scale' show-location='map.showLocation' :longitude='map.longitude' :latitude='map.latitude'
		 :width='map.width' :height='map.height' :controls='map.controls' :markers='map.markers' @regionchange='mapChange'>
			<cover-image src="/static/icon_position.png" class="icon-img"></cover-image>
		     <cover-view class="btn-cover">
				 <cover-view class="btn-left" @tap="handleCancel">取消</cover-view>
				 <cover-view class="btn-right" @tap="handleFinish">完成</cover-view>
			 </cover-view>		
		</map>

		<view class='footer bg-ff font-26' :style="{'height':footHeight}">		
				<view class="jiantou" v-if="showBottom===true">
					<image src="../../static/jiantouixia.png" mode=""></image>
				</view>
				<uni-search-bar :radius="100" @input="addressInput"
				 @confirm="addressConfirm"placeholder="搜索地点" @cancel="addressCancel">
				 </uni-search-bar>
			
			<scroll-view scroll-y class='scroll' :scroll-top='scrollTop' >
				<view class="">
					<view class='padding border-b position-r' v-for='(item, index) in list' :key='index' @click='bindAddress(index)'>
						<view class='row'>{{item.title}}</view>
						<view class='row color-99'>{{item.address}}</view>
						<icon type='success' color='#E74246' size='22' class='icon_circle' v-if='checked === index' />
					</view>
				</view>

			</scroll-view>
		</view>
	</view>
</template>

<script>
    import uniSearchBar  from '@/components/uni-search-bar/uni-search-bar.vue';
	import QQMapWX from'@/libs/qqmap-wx-jssdk.min.js';
	const qqmapsdk = new QQMapWX({key: 'LXCBZ-NNIKD-UZ64F-H6AFI-UNJLH-OCFGE'})
	let cloneList=[];
	export default {
		data() {
			return {
				footHeight:'250px',
				showBottom:false,
				detail: '',
				map: {
					longitude: 113.76927057974245,
					latitude: 34.76670519464811,
					showLocation: true,
					width: 40,
					height: 100,
					scale: 16
				},
				list: [],				
				oftenList: [],
				address: {
					title: '',
					address: ''
				},
				checked: 0,
				scrollTop: 0,
				top:'110upx',
				mapStatus: 1 // 控制选择地址时 地图不加载附近列表
			}
		},
		components:{
			uniSearchBar
		},
		onLoad(options) {	 
			this.map.latitude = options.latitude;
			this.map.longitude = options.longitude;
			this.reverseGeocoder(options.latitude,options.longitude);
			
		},
		methods: {
			reverseGeocoder(latitude,longitude){
				let _this=this;
				qqmapsdk.reverseGeocoder({
					location: {
						latitude:latitude,
						longitude:longitude
					},
					get_poi: 1,
					poi_options: "page_size=20;page_index=1",
					success: function(e) {						
							_this.setData({
								list: e.result.pois
							})
						setTimeout(() => {
							_this.scrollTop = 1
						}, 1000)
					},
					fail: err => {
						console.log(err)
					}
				})
			},
			addressInput(e){				
				this.footHeight='350px';
				this.showBottom=true;
				this.map.height=50;
				cloneList=[].concat(this.list);				
				this.searchList(e.value);
				
			},
			addressConfirm(e){
				this.searchList(e.value);
			},
			addressCancel(){
				this.map.height=100;
			   this.footHeight='250px';
			   this.showBottom=false;			   
			   this.setData({
				   list:cloneList
				  
			   })								
			},
			handleCancel(){
				uni.navigateBack({
					delta: 1
				})
			},
			handleFinish(){
				uni.redirectTo({
					url: '/pages/index/index?address='+this.address.title+'&latitude='+this.map.latitude+'&longitude='+this.map.longitude
				});
				
			},
			searchList(keywords){
			    let that = this;
				qqmapsdk.getSuggestion({
					keyword: keywords,
					success: (res) => {					
							that.list= res.data				
					},
					fail: err => {
						console.log(err)
					}
				})
			},
			getAddress() {
				let _this = this
				uni.getLocation({
					type: 'gcj02',
					success: (res) => {
						let map = _this.data.map;
						map.longitude = res.longitude;
						map.latitude = res.latitude;
						_this.setData({
							map: map,
							position: {
								longitude: res.longitude,
								latitude: res.latitude
							}
						});

						_this.getAddressList(1)

					},
				})
			},

			//通过地图经纬度查询中文地址以及周边地址
			getAddressList(s = 0) {			
                this.reverseGeocoder(this.position.latitude,this.position.longitude)
				
			},
			//地图移动或者变换就找到中心点经纬度
			mapChange(e) {
				let _this = this;
				clearTimeout(this.timer)
				this.timer = setTimeout(() => {
					if (e.type == 'end') {
						_this.mapCtx = uni.createMapContext('map')
						_this.mapCtx.getCenterLocation({
							success: res => {
								_this.setData({
									position: {
										latitude: res.latitude,
										longitude: res.longitude
									}
								});

								if (_this.mapStatus) { // 防止地图点击时 进行多次加载
									_this.getAddressList(1)
								} else {
									_this.mapStatus = 1
								}
							}
						})
					}
				}, 200)

			},
			bindAddress(index) {
				let list = this.list
				let map = this.map
				map.latitude = list[index].location.lat
				map.longitude = list[index].location.lng
				this.setData({
					map: map,
					checked: index,
					address: list[index],
					mapStatus: 0
				})
			},
			setData(obj) {
				Object.assign(this, obj)
			},


			addressSearch() {
				uni.navigateTo({
					url: '/pages/address-search/address-search'
				})
			},
			
		}
	}
</script>

<style lang="scss">
	/**app.wxss**/
	page,
	view,
	scroll-view,
	swiper,
	block,
	icon,
	text,
	rich-text,
	button,
	input,
	label,
	picker,
	picker-view,
	slider,
	textarea,
	navigator,
	image,
	video,
	map,
	video {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
	.btn-cover{
		position:absolute;
		top:140upx;
		width:100%;
		display: flex;
		flex-direction: row;
		justify-content: space-between;	
	}
    .btn-left,.btn-right{
		padding:20upx 40upx;
		color:#FFFFFF;		
		border-radius:4px;
	}
	.btn-left{
		background-color:#C0C0C0;
		margin-left:40upx;
		
	}
	.btn-right{
		background-color:#E74246;
		margin-right:40upx;
		
	}
	page {
		//background: #F6F6F6;
		font-size: 32rpx;
	}

	image {
		display: block;
	}

	.item,
	.item-forward {
		background: #fff;
		padding: 25rpx 90rpx 25rpx 25rpx;
		position: relative;
		line-height: 46rpx;
	}

	.item-forward::before {
		content: '';
		width: 20rpx;
		height: 20rpx;
		border-top: 2px solid #a9a9a9;
		border-right: 2px solid #a9a9a9;
		border-radius: 2px;
		position: absolute;
		top: 50%;
		right: 35rpx;
		transform: rotate(45deg) translateY(-50%);
	}

	.item image,
	.item-forward image {
		width: 46rpx;
		height: 46rpx;
		display: block;
		margin-right: 10rpx;
		position: relative;
	}

	.active {
		background: #eee;
	}

	.row,
	.item,
	.item-forward,
	.coupons {
		display: flex;
		width: 100%;
	}

	.row-wrap {
		flex-wrap: wrap;
	}

	.col,
	.coupons .left {
		flex: 1;
		display: block;
		width: 100%;
	}

	.col-center {
		height: 100%;
		display: flex;
		align-items: center;
	}

	.float-r {
		float: right;
	}

	.padding {
		padding: 20rpx 25rpx;
	}

	.padding-t {
		padding-top: 20rpx;
	}

	.padding-b {
		padding-bottom: 20rpx;
	}

	.padding-l {
		padding-left: 25rpx;
	}

	.padding-r {
		padding-right: 25rpx;
	}

	.padding-tb {
		padding-top: 20rpx;
		padding-bottom: 20rpx;
	}

	.padding-lr {
		padding-left: 25rpx;
		padding-right: 25rpx;
	}

	.margin {
		margin: 20rpx 25rpx;
	}

	.margin-t {
		margin-top: 20rpx;
	}

	.margin-b {
		margin-bottom: 20rpx;
	}

	.margin-tb {
		margin-top: 20rpx;
		margin-bottom: 20rpx;
	}

	.margin-lr {
		margin-left: 25rpx;
		margin-right: 25rpx;
	}

	.border,
	.border-t,
	.border-r,
	.border-b,
	.border-l {
		position: relative;
	}

	.border {
		border: .5px solid #eee;
	}

	.border-t::after,
	.border-r::after,
	.border-b::after,
	.border-l::after {
		content: '';
		position: absolute;
		/*background: #eee;*/
		background: linear-gradient(to top, #eee .7px, transparent .7px);
	}

	.border-t::after,
	.border-b::after {
		height: 1px;
		left: 25rpx;
		right: 25rpx;
		top: 0;
	}

	.border-b::after {
		top: auto;
		bottom: 0;
	}

	.border-l::after,
	.border-r::after {
		width: 1px;
		top: 0;
		bottom: 0;
		left: 0;
		background: linear-gradient(to right, #eee .7px, transparent .7px);
	}

	.border-r::after {
		left: auto;
		right: 0;
	}

	.ellipsis-1 {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.bg {
		background: #E74246;
	}

	.bg-ff {
		background: #fff;
	}

	.color {
		color: #E74246;
	}

	.color-00 {
		color: #000;
	}

	.color-ff {
		color: #fff;
	}

	.color-99,
	.icon_img_tip {
		color: #999;
	}

	.color-6c {
		color: #6c6c6c;
	}

	.text-right {
		text-align: right;
	}

	.font-26 {
		font-size: 26rpx;
	}

	.position-r {
		position: relative;
	}





	page {
		position: relative;
	}

	.map,
	map {
		width: auto;
		height: auto;
		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		bottom: 210px;
	}

	.map {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.header {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		height: 100px;
		overflow: hidden;
	}

	.icon_search {
		margin-top: 20rpx;
	}

	.btn {
		line-height: 25px;
		border-radius: 4rpx;
		margin-top: -5rpx;
	}

	.footer {
		position: fixed;
		left: 0;
		right: 0;
		bottom: 0;
		.jiantou{
			display: flex;
			justify-content: center;
			
			image{
				width:40upx;
				height:40upx;
				
			}
		}
		
	}

	.foot-border {
		border-bottom: 1px solid #eee;
		line-height: 32rpx;
	}

	.foot-border .padding {
		padding: 25rpx;
	}

	.foot-active {
		color: #E74246;
		position: relative;
	}

	.foot-active::after {
		content: '';
		position: absolute;
		height: 2px;
		background: #E74246;
		border-radius: 2px;
		bottom: 0;
		width: 5em;
		left: 50%;
		transform: translateX(-50%);
	}

	.scroll {
		position: absolute;
		left: 0;
		right: 0;
	    top: 130upx;
		bottom: 0;
	}

	.scroll .padding {
		padding-right: 40px;
	}

	.scroll .icon_circle {
		position: absolute;
		right: 25rpx;
		top: 50%;
		transform: translateY(-50%);
	}

	.icon_img_tip {
		padding: 20rpx 0;
	}

	.icon-position {
		position: relative;
		top: 50%;
		left: 50%;
		width: 36px;
		height: 36px;
		transform: translate(-50%, -50%);

		.icon-img {
			width: 36px;
			height: 36px;
			display: block;
			position: fixed;
			top: 100px;
			left: 100px;
		}
	}

	.icon-img {
		width: 36px;
		height: 36px;
		display: block;
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		margin-top: -70px;
	}
	


</style>
