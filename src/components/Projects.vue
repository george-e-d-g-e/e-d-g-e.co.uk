<template>
	<main ref="main">

		<!-- TO DO: 
			Hide and move elements and show the projects titles and meta. When selectProduct display elements again
			(When mouseover dots change state)
		-->

		<section
			v-if="selectedProject"
		>
			 <div 
			 	class="hero-container" 
			 	:class="{active : menuActive}">
			 	<div class="hero-crop" :style="cropRotate">
			 		<!-- 
			 			TO DO: 
			 			[ ] Change image only once closed animation call back
			 		-->
				 	<img 
				 		:src="selectedImage"
				 		:style="imgRotate"
				 		>	 
			 	</div>
			 </div>
			<div class="info-crop">
				<section 
				ref="proj"
				v-for="project in projects"
				:key="project.id"
				class="infomation"
				:class="getActive(project.id)"
				:style="translateOffset(project.id)"
				>

					<h2> {{ project.title }} </h2>
					<p class="tagline"> {{ project.info }} </p>
					<p class="meta"> {{ project.meta }} </p>
					
					<button 
						class="cta"
						:href="'/projects' + project.url"
					> View </button>

				</section>
			</div>

			<!-- <section 
				class="infomation"
				>

				<h2> {{ selectedProject.title }} </h2>
				<p class="tagline"> {{ selectedProject.info }} </p>
				<p class="meta"> {{ selectedProject.meta }} </p>
				
				<button 
					class="cta"
					:href="'/projects' + selectedProject.url"
				> View </button>

			</section> -->

		</section>

		<nav id="dots">

		<!-- TO DO: 
		[] Create and import DotsNav componet
		 -->
			<div 
				class="container"
				@mouseover="menuActive = true"
				@mouseleave="deActivate()"
			>

			<!-- TO DO: 
				[] show active
				[] more juicy animation
				[] scale & slide with mouse
			-->
				<div
					v-for="project in projects"
					class="dot"
					:key="project.id"
					@mouseover="changeTo(project)"
				>
				<!-- @mouseover="selectedProject = project;" -->
				</div> 
			</div>
		</nav>

	</main>
</template>

<script>
	export default {
		name: "Projects",
		data: function() {
			return {

				/* TO DO:
					investigate markdown https://www.npmjs.com/package/vue-markdown and section of data to more managible chunks
				*/
				imagesURL : '@/assets/images/',
			  	projects: [
			  		{ 	id: 0,
			  			title: 'Ritual',
			  			img: require('@/assets/images/ritual.jpg'),
			  			info: 'A ceromony of the future.',
			  			meta: 'Texas, USA, 2018',
			  			url: '/ritual',
			  			offset: 0
			  		},
			  		{	id: 1,
			  			title: 'Gestalt',
			  			img: require('@/assets/images/gestalt.jpg'),
			  			info: 'Simulating the phenomon of paradolia.',
			  			meta: 'Berlin, Germany, 2016',
			  			url: '/gestalt',
			  			offset: 0
			  		},
			  		{	id: 2,
			  			title: 'Ethereal',
			  			img: require('@/assets/images/ethereal.jpg'),
			  			info: 'Drawing with lasers.',
			  			meta: 'Belfast, UK, 2017',
			  			url: '/Ethereal',
			  			offset: 0
			  		},
			  		{	id: 3,
			  			title: 'St Annes Chapel',
			  			img: require('@/assets/images/stannes.jpg'),
			  			info: '16th century architecture seen differently.',
			  			meta: 'Bidiford, UK, 2015',
			  			url: '/stannes',
			  			offset: 0
			  		},
			  	],
			  	selectedProject: null,
			  	selectedImage: "",
			  	selectedOffset: 0,
			  	centerOffset: 0,
			  	heroRotation: 0,
			  	menuActive: false,
			  	needsUpdating: false,

			}
		},
		props: {
			heading: String
		},
		methods: {
			changeTo: function(p){
				if (p != this.selectedProject){
					this.selectedProject = p;
				}
			},
			updateOffsets: function () {

				let w = 0; /* Defualt */
				if(this.$refs.proj){ /*Check if exist*/
					let vw = this.$refs.main.clientWidth;   /* view width */
					for (var i = 1 ; i < this.projects.length; i++) {					
						let pw = this.$refs.proj[i-1].clientWidth; 	/* prev section width */
						w += (pw / vw) * 100; 
						w += 7.5;
						this.projects[i].offset = w;						
					}

					// center 
					if (this.selectedProject){
						let sw = this.$refs.proj[this.selectedProject.id].clientWidth; 
						this.centerOffset = (vw/2) - (sw/2);
						this.centerOffset = (this.centerOffset / vw) * 100; /*Conver to %*/
						this.centerOffset -= 7.5;
					}	
				}
	    	},
	    	getActive: function(id) {
	    		this.needsUpdating = true;
	    		return {
	    			'active' : this.menuActive, 
	    			'selected' : (this.selectedProject.id === id)
	    		};
	    	},
	    	deActivate: function() {
	    		this.menuActive = false;
	    		if(this.selectedProject){
	    			this.selectedImage = this.selectedProject.img;
	    			this.heroRotation = (Math.random() * 10) - 5;
	    			console.log('change image');
	    		}
	    		
	    	},
	    	translateOffset: function (id) {
	    		let x = 7.5 + (this.projects[id].offset - this.selectedOffset);
	    		let y = 0;
	    		if (this.selectedProject){
		    		if (this.selectedProject.id == id){
		    			y = 0;
		    		}
		    	}
	    		return {transform: 'translate(' + x + 'vw, '+ y +'vh)'};
	    	}
	    }, 
	    computed: {
	    	imgRotate: function () { 
	    		return {transform: 'rotate(' + this.heroRotation + 'deg)'};
	    	},
	    	cropRotate: function() { 
	    		return {transform: 'rotate(' + -this.heroRotation + 'deg)'};
	    	}
	    },
	    watch: {
	    	needsUpdating: function () {
	    		this.updateOffsets();
	    		this.selectedOffset = this.selectedProject.offset - this.centerOffset;
	    		this.needsUpdating = false;
	    	}
	    }
	};
</script>

<style scoped lang="scss">

	@font-face {
	  font-family: Borda;
	  src: url(~@/assets/fonts/Borda_Medium.ttf);
	}

	main {
		font-family: Borda;
	}
	
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
		transition: height 1s, top 1s;
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
			transition: top 1s;
			// transition-timing-function: cubic-bezier(0.34, 0.12, 0.26, 1.67);
		}
	}

	.hero-container.active {
		.hero-crop{
			height: 0vh;
			top: 25vh;
			img {
				top: -25vh;
			}
		}
	}

	// Infomation 
	.info-crop{
		width: 100vw;
		height: 50vh;
		overflow: hidden;
		position: absolute;
	}

	.infomation{
		width: 85vw;
	    margin: auto;
	    max-width: 600px;
	    position: absolute; 
	    opacity: 0.0;
	    // border: red solid 1px; /* For debugging */
	    transition: width 1s, transform 1s, opacity 1s;

	    h2 {
			margin: 0px;
			font-size: 3em;
			transform: translateY(5px) scale(0.8);
			transform-origin: 0% 100%;
			transition: transform 0.5s  ease-in-out, margin 0.5s;
		}

		.tagline {
			font-size: 1.3em;
			opacity: 0;
			max-height: 0px;
			margin: 0px;
			overflow: hidden;
			transform: translateX(100px);
			transition: opacity 0.5s ease-in-out 0.2s, transform 0.5s ease-in-out 0.2s, max-height 0.5s, margin 0.5s;
		}

		.meta {
			font-size: 0.8em;
		}

		button {
			opacity: 0;
			display: none;
			transition: opacity 0.5s;
		}
	}

	.infomation.active {
		opacity: 0.5;
		width: auto;
		max-width: 85vw;
		.tagline{
			width: 0px;
		}	
	}

	.infomation.active.selected {
		opacity: 1.0;
		width: auto;
		max-width: 85vw;
		h2 {
			margin: 0px;
		}
		.tagline{
			width: 0px;
			opacity: 0;
			max-height: 0px;
			margin: 0px;
			transform: translateX(100px);
		}
		button {
			opacity: 0;
			display: block;
		}	
	}

	.infomation.selected{
		opacity: 1;	
		h2 {
			margin: 0px 0px 0.5em 0px;
			transform: translateY(0px);
		}
		.tagline{
			margin: 1em 0px;
			max-height: 1.3em;
			transform: translateX(0px);
			opacity: 1;
		}
		button {
			opacity: 1;
			display: block;
		}
	}



	// Dot menu

	#dots {
		position: absolute;
		bottom: 20px;
		margin: 0px auto;
		min-width: 100%; 

		.container {
			display: flex;
			justify-content: center;
			align-content: center;
			margin: auto;
			max-width: 100%;
			transform: translate(0px,10px);
			transition: transform .5s;
			transition-timing-function: cubic-bezier(0.34, 0.12, 0.26, 1.67);
		}

		.container:hover {
			transform: translate(0px,0px);
		}

		.dot {
			position: relative;
			// margin: auto;
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
	}

	#dots:hover {

		.dot:after {
			opacity: 0.6;
		}

		.dot:hover:after  {
			transform: translateY(-10px) scale(1.1,1.6);
			opacity: 1;
		}
	}

</style>