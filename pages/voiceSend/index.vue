<template>
	<view class="voice-record" @tap="showPicker">
		<slot></slot>
		<view class="mask" @tap.stop="closePicker" v-if="isShow" @touchmove.stop.prevent="moveHandle"></view>
		<view class="conbox record" :class="{'pickerShow':isShow}">
			<view class="time">
				{{showRecordTime}}
			</view>
			<view class="c999">
				最短{{minTime}}秒，最长{{maxTime}}秒
			</view>
			<view class="record-box" @touchstart="start" @longpress="record" @touchend="end"  @touchmove.stop.prevent="moveHandle">
			<span class="stop" @touchstart.stop="stopVoice" v-if="voicePath && playing==1"></span>
			<span class="paly" @touchstart.stop="playVoice" v-if="voicePath && playing==0"></span>
			<canvas class="canvas" canvas-id="canvas">
				<span class="recording"></span>
			</canvas>
			<span class="confirm" @touchstart.stop="okClick" v-if="voicePath"></span>
		</view>
		<view class="c666 fz32 domess">长按录音</view>
	</view>
	</view>
</template>

<script>
	export default {
		name: 'voice-record',
		props: {
			voicePath: { //默认地址
				type: String,
				default: '' 
			},
			maxTime: {
				type:Number,	
				default:15
			},
			minTime: {
				type:Number,
				default: 5
			},
		},
		data(){
			return{
				isShow:false,
				recordTime:0,
			}
		},
		computed:{
			showRecordTime(){
				var strs = "";
				var m = Math.floor(this.recordTime/60);
				if(m<10) strs = "0"+m;
				
				var s = this.recordTime%60;
				strs += (s<10) ? ":0"+s : ":"+s;
				
				return strs
				
			}
		},
		methods: {
			moveHandle() {
				return false;
			},
			//组件数据初始化 进入时、关闭是调用初始化
			initValue() {
				
			},
			//显示组件
			showPicker(){
				setTimeout(() => {
					this.isShow = true;
				},100);
			},
			//关闭组件
			closePicker(){
				this.isShow = false;
				//点遮罩 点取消关闭说明用户不想修改，所以这里对数据进行初始化
				this.initValue();
				this.stopVoice();
			},
			//点击确定
			okClick(){
				this.$emit('okClick',this.voicePath)
				this.closePicker();
			}
		}
	}
</script>
 
 <style lang="scss">
 	.voice-record {
 
        .mask {
 		  position: fixed;
 		  z-index: 1000;
 		  top: 0;
 		  right: 0;
 		  left: 0;
 		  bottom: 0;
 		  background: rgba(0, 0, 0, 0.6);
 		}
 
         .conbox{
 			transition: all .3s ease;
 			transform: translateY(100%);
 			&.pickerShow{
 			   transform:translateY(0);
 			} 
 			
 			position: fixed;
 			z-index: 1000;
 			right: 0;
 			left: 0;
 			bottom: 0;
 			background: #fff;
 		}
 		
 		.c666{color:#666;}
 		.c999{color:#999;}
 		.fz28{font-size: 28upx;}
 		.fz32{font-size: 32upx;}
 		
 		
         .record{		
 			text-align: center;
 			
 			.time {
 				text-align: center;
 				font-size: 60upx;
 				color: #000;
 				line-height: 100upx;
 				margin-top:50upx;
 			}
 			.domess{margin-bottom:50upx;}
 			
 			
 			.record-box {
 				display: flex;
 				flex-direction: row;
 				justify-content: center;
 			}
 			canvas {
 				margin:10upx 60upx;
 				position: relative;
 				width: 200upx;
 				height: 200upx;
 				z-index: 10;			
 				.recording{
 					position: absolute;
 					top: 20upx;
 					left: 20upx;
 					width: 160upx;
 					height: 160upx;
 					border: 1px dashed #fe3b54;
 					border-radius: 100upx;
 					background:#fe3b54 url(../../static/jsfun-record/recording.png) no-repeat 50% 50%;
 					background-size: 50% 50%;
 					z-index: 100;
 				}
 			}
 			
 			.btncom{
 				margin-top: 70upx;
 				width: 80upx;
 				height: 80upx;	
 				border-radius: 80upx;
 			}
 			.stop{
 				 @extend .btncom;
 				background:url(../../static/jsfun-record/stop.png) no-repeat;
 				background-size: 100% 100%;
 			}
 			.paly{
 				 @extend .btncom;
 				background:url(../../static/jsfun-record/play.png) no-repeat;
 				background-size: 100% 100%;
 			}			
 			.confirm{
 				 @extend .btncom;
 				background:url(../../static/jsfun-record/confirm.png) no-repeat 100% 100%;
 				background-size: 100% 100%;
 			}
 			
 			
 		} 
 
 	}
 </style>