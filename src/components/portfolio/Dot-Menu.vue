<template>
	<nav id="dot-menu">
		<div 
			class="container"
			:class="{active: choosing}"
			@mouseover="emitEnter"
			@mouseleave="emitLeave"
			@mousemove="mousingDots"
			@touchstart="emitEnter"
			@touchend="emitLeave"
			@touchmove="touchingDots"
			>

			<slot></slot>

		</div>
	</nav>
</template>

<script>
	export default {
  		name: "DotMenu",
  		props:{ 
  			selected : Number
  		},
  		data: function() {
  			return{
  				choosing: false,
  				newSelected: null,
  			}
  		},
  		methods: {
  			emitEnter(){
  				this.choosing = true;
  				this.$emit('enter');
  			},
  			emitLeave(){
  				this.choosing = false;
  				this.$emit('leave');
  			},
  			mousingDots(event){
  				let x = event.clientX;
				let y = event.clientY;
				let target = parseInt(document.elementFromPoint(x, y).getAttribute("num"));
				if(target != this.newSelected) {
					this.newSelected = target; 
					if (this.newSelected >= 0){
						this.$emit('update:selected', this.newSelected)  /*to work with sync modifier*/
					}
				} 
  			},
  			touchingDots(event){
  				let x = event.touches[0].clientX;
				let y = event.touches[0].clientY;
				let target = parseInt(document.elementFromPoint(x, y).getAttribute("num"));
				if(target != this.newSelected) {
					this.newSelected = target; 
					if (this.newSelected >= 0){
						this.$emit('update:selected', this.newSelected)  /*to work with sync modifier*/
					}
				} 
  			}
  		}
	};
</script>

<style scoped lang="scss">
	#dot-menu {
		position: absolute;
		bottom: 0px;
		margin: 0px auto;
		min-width: 100%; 

		.container {
			display: flex;
			justify-content: center;
			align-content: center;
			padding: 10px 0 30px 0;
			margin: auto;
			max-width: 100%;
			transform: translate(0px,0px);
			transition: transform .5s;
			transition-timing-function: cubic-bezier(0.34, 0.12, 0.26, 1.67);
		}

		.container.active {
			transform: translate(0px,0px);
		}
	}

	/* @media ( min-width: 750px ) {
		#dot-menu:hover {
			.dot:after {
				opacity: 0.6;
			}

			.dot:hover:after  {
				transform: translateY(-20px) scale(1.1,1.6);
				opacity: 1;
			}
		}

	}
	
	#dot-menu.active{
		.dot:after {
			transform: translateY(-10px) scale(1.0);
			opacity: 0.6;
		}

		.dot:active:after  {
			transform: translateY(-10px) scale(1.1,1.6);
			opacity: 1;
		}
	} */
</style>