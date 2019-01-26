<template>
	<div class="hero-container">
		<transition name="hero-image">
			<div 
				class="hero-crop" 
				:style="cropRotate"
				v-if="!choosing && first"
				>
			 	<img 
			 		:src="currentImage"
			 		:style="imageRotate"
			 		key="image"
			 		>
		 	</div>
	 	</transition>
	 </div>
</template>
<script>
	export default {
  		name: "HeroImage",
  		props: {
  			image: String,
  			choosing: Boolean,
  		},
  		data: function() {
			return {
				currentImage: '',
	  			heroRotation: 0,
	  			first: false,
	  		}
  		},
	    watch: {
	    	image() {
	    		if (this.image != this.currentImage){
	    			/* Wait for choosing to finish */
	    			this.currentImage = this.image;
	    			this.first = true; // 
	    			this.heroRotation = (Math.random() * 10) - 5;
	    		}
	    	},
	    },
	    computed: {
	    	imageRotate() { 
	    		return {transform: 'rotate(' + this.heroRotation + 'deg)'};
	    	},
	    	cropRotate() { 
	    		return {transform: 'rotate(' + -this.heroRotation + 'deg)'};
	    	},
	    },
	};
</script>

<style scoped lang="scss">
	
	//  Hero Image

	.hero-container{
		width: 100%;
		height: 50vh;
		overflow: hidden;	
	}

	.hero-crop{
		position: relative;
		height: 40vh;
		width: 120%;
		top: 5vh;
		right: 10%;
		overflow: hidden;
		transform: rotate(10deg);
		/* transition: height 1s, top 1s; */
		// transition-timing-function: cubic-bezier(0.34, 0.12, 0.26, 1.67);
		img{
			position: relative;
			background: #616161;
			width: 100%;
	    	height: 50vh;
	    	top: -5vh;
	    	object-fit: cover;
	    	object-position: center;
			transform: rotate(-10deg);
			// transition-timing-function: cubic-bezier(0.34, 0.12, 0.26, 1.67);
		}
	}

	//  Transitions 
	.hero-image-enter-active, .hero-image-leave-active{
		transition: height 1s, top 1s;
		img {
			transition: top 1s;
		}
	}
	// Open
	.hero-image-enter-to, .hero-image-leave{
		height: 40vh;
		top: 5vh;
		img {
			top: -5vh;
		}
	}
	// Closed
	.hero-image-leave-to, .hero-image-enter{
		height: 0vh;
		top: 25vh;
		img {
			top: -25vh;
		}
	}

</style>