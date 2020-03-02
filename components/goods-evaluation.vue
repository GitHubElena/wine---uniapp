<template>
	<view class="content">
		<view class="evaluation" v-for="(item,index) in list">
			<view class="e-left">
				 <view class="username">
				 	{{item.username}}
				 </view>
				 <view class="word">
				 	{{item.word}}
				 </view>
				 <view class="date" >
				 	{{item.date}}
				 </view>
			</view>
			<view class="e-right">
				<view class="count" >
					<image src="../static/heart.png" mode=""></image>
					<image src="../static/heart.png" mode=""></image>
					<image src="../static/heart.png" mode=""></image>
					<image src="../static/heart.png" mode=""></image>
					<image src="../static/heart.png" mode=""></image>
				</view>
				
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[
					{
						username:'wine+873r3',
						word:'不错哦',
						star:5,
						date:'2019-07-18 22:40:49'
					},
						{
							username:'wine+873r5y53',
							word:'不错哦',
							star:5,
							date:'2019-03-13 13:49:43'
						},
					
				]
			};
		}
	}
</script>

<style lang="scss">
  .content{
	  .evaluation{
		  padding:20upx;
		  display: flex;
		  flex-direction: row;
		  justify-content: space-between;
		  border-bottom:1px solid #EEEEEE;
		  .e-left{
			  font-size:10px;
			  display: flex;
			  flex-direction: column ;
			  justify-content: flex-start;
			  .word{
				  margin:20upx 0upx;
			  }
			  .date{
				  color:#C0C0C0;
			  }
		  }
		  .e-right{
			  .count{
				  image{
					  width:40upx;
					  height:40upx;
				  }
			  }
		  }
	  }
  }
</style>
