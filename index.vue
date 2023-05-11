<template>
	<view class="content">
		<div class="square" @mouseenter='enter' @mouseleave='leave' @click='scale'>
			<canvas id="canvas" style="width: 100%;height: 100%;"></canvas>
		</div>
		<uni-popup ref="popup" type="share">
			<uni-popup-dialog style="margin: 0 auto;" ref="inputClose" mode="input" title="输入内容" :value="input"
				:before-close="true" placeholder="请输入内容" @close="close" @confirm="confirm"></uni-popup-dialog>
		</uni-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				square: null,
				canvas: null,
				ctx: null,
				flag: false,
				input: '面试官，你好，我叫'
			}
		},
		mounted() {
			this.getSquare()
		},
		methods: {
			getSquare() {
				this.square = document.querySelector('.square')
				this.canvas = document.querySelector('canvas')
			},
			async enter() {
				this.square.style.background = 'red'
				this.ctx = this.canvas.getContext('2d')
				this.ctx.beginPath()
				this.ctx.strokeStyle = 'white'
				this.isScale()
				this.ctx.closePath()
				this.ctx.stroke()
				if (this.flag) {
					await this.delay(1000)
					this.square.classList.add('opacity')
					this.$refs.popup.open()
				}
			},
			leave() {
				this.square.style.background = 'aqua'
				this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
			},
			scale() {
				this.flag = true
				this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
				this.square.style.width = '100px'
				this.square.style.height = '100px'
				this.enter()
			},
			isScale() {
				if (this.flag) {
					this.ctx.lineWidth = 5
					this.ctx.arc(50, 50, 50, 0, 2 * Math.PI, false)
				} else {
					this.ctx.lineWidth = 10
					this.ctx.arc(100, 100, 100, 0, 2 * Math.PI, false)
				}
			},
			delay(interval) {
				typeof interval !== 'number' ? (interval = 1000) : null
				return new Promise(resolve => {
					setTimeout(() => {
						resolve()
					}, interval)
				})
			},
			confirm() {
				this.finally()
			},
			close() {
				this.finally()
			},
			finally() {
				this.$refs.popup.close()
				this.flag = false
				this.square.style.width = '200px'
				this.square.style.height = '200px'
				this.square.classList.remove('opacity')
			}
		}
	}
</script>

<style>
	.content {
		width: 100vw;
		height: calc(100vh - 44px);
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.square {
		height: 200px;
		width: 200px;
		background: aqua;
		transition: all 0.5s ease;
	}

	.opacity {
		opacity: 0;
	}
</style>
