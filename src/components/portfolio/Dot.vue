<template>
	<div class="dot" 
		@mouseover="emitId"
		@touchmove="emitId"
		>
	</div>
</template>

<script>
	export default {
  		name: "Dot",
  		methods: {
  			emitId(event){
  				// console.log(window.touchEvent.targetTouches);
  				this.$emit('hover');
  			} 
  		}
	};
</script>

<style scoped lang="scss">
	.dot {
		position: relative;
		bottom: 0px;
		padding: 10px;
	}

	.dot:after {
		content: "";
		display: inline-block;
		background: #000;
		opacity: 0.5;
		height: 10px;
		width: 5px;
		bottom: 0px;
		border-radius: 0.1em;
		transition: transform .2s, opacity .2s;
		transition-timing-function: cubic-bezier(0.34, 0.12, 0.26, 1.67);
	}

	.selected.dot:after  {
		transform: translateY(-5px) scale(1.0);
		opacity: 0.65;
	}

	.active{
	 	.dot:after {
			transform: translateY(-10px);
			opacity: 0.5;
		}
		.selected.dot:after  {
			transform: translateY(-20px) scale(1.1,2.0);
			opacity: 1;
		}
	}

	

	

</style>