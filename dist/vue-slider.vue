<template>
	<div class="slider" ref="mySlider">
		<div class="left-btn" @touchstart.prevent="btnFun(2)">
			<img v-if="myPosition.isBtn==2" :src="orange"/>
			<img v-else :src="white"/>
		</div>
		<div class="propo"></div>
		<div class="propo-bg"></div>
		<div class="right-btn" @touchstart.prevent="btnFun(1)">
			<img v-if="myPosition.isBtn==1" :src="orange"/>
			<img v-else :src="white"/>
		</div>
	</div>
</template>
<script type="text/javascript">
import orange from './assets/orange.png'
import white from './assets/white.png'
	export default {
		name: 'slider',
		props:{
			"valueFun":{
				type:Function,
				required: true,
			},
			"max":{
				type:Number,
				default:100
			},
			"min":{
				type:Number,
				default:0
			}
		},
		data(){
			return{
				myPosition:{
					left:0,
					right:0,
					now:0,
					isBtn:0,
					propoWidth:0
				},
				myDefault:null,
				white,
				orange,
			}
		},
		methods:{
			btnFun(index){
				this.myPosition.isBtn = index
			}
		},
		mounted(){
			//滑块
			let that = this
			let mySlider = this.$refs.mySlider
			let propo = mySlider.children[1]
			let rightBtn = mySlider.children[3]
			let leftBtn = mySlider.children[0]
			let myWidth = 0

			const elementLeft = (e) => { //计算x坐标
				var offset = e.offsetLeft;
				if(e.offsetParent != null) offset += elementLeft(e.offsetParent);
				return offset;
			}

			const myCount = () => { //计算滑动
				if(this.myPosition.right>this.myPosition.left){ //判断滑动范围
					this.myPosition.propoWidth = this.myPosition.right - this.myPosition.left
					propo.style.width = this.myPosition.propoWidth+'%'
					propo.style.left = this.myPosition.left + '%'
					this.valueFun(parseInt(this.myPosition.left),parseInt(this.myPosition.right),parseInt(this.myPosition.propoWidth))
				}else if(this.myPosition.right<this.myPosition.left){
					this.myPosition.propoWidth = this.myPosition.left - this.myPosition.right
					propo.style.width = this.myPosition.propoWidth+'%'
					propo.style.left = this.myPosition.right+'%'
					this.valueFun(parseInt(this.myPosition.right),parseInt(this.myPosition.left),parseInt(this.myPosition.propoWidth))
				}else if(this.myPosition.right==this.myPosition.left){//按钮位置滑到最大值或者最小值
					this.myPosition.propoWidth = this.myPosition.left - this.myPosition.right
					propo.style.width = this.myPosition.propoWidth+'%'
					propo.style.left = this.myPosition.right+'%'
					this.valueFun(parseInt(this.myPosition.right),parseInt(this.myPosition.left),parseInt(this.myPosition.propoWidth))
				}
				
			}

			this.myDefault = () => {//初始化
				this.myPosition.right = this.max
				this.myPosition.left = this.min
				
				if(this.max > this.min){
					this.myPosition.propoWidth = this.max - this.min
					propo.style.left = this.myPosition.left + '%'
				}else{
					this.myPosition.propoWidth = this.min - this.max
					propo.style.left = this.myPosition.right + '%'
				}
				
				propo.style.width = this.myPosition.propoWidth+'%'
				leftBtn.style.left = this.myPosition.right + '%'
				rightBtn.style.left = this.myPosition.left + '%'
				this.valueFun(this.myPosition.left,this.myPosition.right,this.myPosition.propoWidth)
			}

			let mySliderX = elementLeft(mySlider) //滑动块x坐标

			mySlider.addEventListener('touchmove',(e)=>{ //屏幕滑动事件
				let pageX = e.touches[0].pageX-mySliderX //获取滑动x坐标
				myWidth = (pageX/mySlider.offsetWidth)*100 //计算百分比
				if(myWidth>100){ //判断不超出范围
					myWidth=100
				}else if(myWidth<0){
					myWidth=0
				}
				if(this.myPosition.isBtn == 1){//判断焦点
					this.myPosition.left = myWidth
					rightBtn.style.left = myWidth+'%' 
				}else if(this.myPosition.isBtn == 2){
					this.myPosition.right = myWidth
					leftBtn.style.left = myWidth+'%' 
				}
				myCount()
				e.preventDefault()

			})

			mySlider.addEventListener('touchstart',(e)=>{//屏幕触摸事件
				let touchX = e.touches[0].pageX-mySliderX
				let btnWidth = (leftBtn.offsetWidth/mySlider.offsetWidth)/2*100 //计算按钮宽度
				this.myPosition.now = (touchX/mySlider.offsetWidth)*100
				mySliderX = elementLeft(mySlider) //滑动块x坐标
				// if(this.myPosition.now <= this.myPosition.left+btnWidth&&this.myPosition.now >= this.myPosition.left-btnWidth){ //计算区间 获取焦点
				// 	this.myPosition.isBtn = 1
				// }else if(this.myPosition.now <= this.myPosition.right+btnWidth&&this.myPosition.now >= this.myPosition.right-btnWidth){
				// 	this.myPosition.isBtn = 2
				// }else{
				// 	this.myPosition.isBtn = 0
				// }
			})

			this.myDefault()
		},
		watch:{
			min(New,old){
				this.myDefault()
			},
			max(New,old){
				this.myDefault()
			}
		}
	}
</script>
<style lang="scss">
	.slider{
			width: 100%;
			position:relative;
			height: 0.5rem;
			.left-btn,.right-btn{
				position: absolute;
				top: 0.04rem;
				transform:translate(-50%,0);
				padding: 0 0.3rem 0.3rem 0.3rem;
				img{
					width: 0.335rem;
				}
				// &:before{
				// 	content: ""; 
				// 	display: block;
				// 	width: 0; 
				// 	height: 0.4rem;
				// 	border-left: 0.17rem solid transparent; 
				// 	border-right: 0.17rem solid transparent; 
				// 	border-bottom: 0.17rem solid #B6B6B6; 
				// }
				// &:after{
				// 	content: "";
				// 	display: block;
				//     height: 0.30rem;
				//     width: 0.335rem;
				//     background: #B6B6B6;
				//     border-bottom-left-radius: 0.1rem;
				//     border-bottom-right-radius: 0.1rem;
				// }
			}
			.propo{
				width: 0%;
				height: 0.04rem;
				background: #FFD00D;
				position: absolute;
			    top: 0;
			    left: 0;
			    z-index: 2;
			}
			.propo-bg{
				background: #B6B6B6;
				border-radius: 0.04rem;
				height: 0.04rem;
				width:100%;
				position: absolute;
			    top: 0;
			    left: 0;
			    z-index: 1;
			}
			.active{
				&:before{
					border-bottom-color: #FF8C30
				}
				&:after{
					background: #FF8C30;
				}
			}
	}
</style>
