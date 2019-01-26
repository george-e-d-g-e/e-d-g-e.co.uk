<template>
	<main ref="main">

		<hero-image 
			:class="{active : choosing}"
			:choosing="choosing"
			:image="getSelectedImage"
			> 
		</hero-image>	


		 <div class="info-crop">
			<hero-text
				v-for="project in projects"
				:key="project.id"
				:project = "project"
				:class="{ active : choosing, selected : (project.id == selected) }"
				:style="translateOffset(project.id)"
				ref="herotext"
				>
			</hero-text>
		</div>
		<p class="instructions" v-if="instruct">{{ getInstructions() }}</p>
		<dot-menu
			@enter="choosing = true"
			@leave="choosing = false"
			:selected.sync="selected"
			> 
			<dot 
				v-for="project in projects"
				:key="project.id"
				:class="{ selected : (project.id == selected) }"
				:num="project.id"
				>
			</dot>
		</dot-menu> 

	</main>
</template>

<script>

import DATA from "@/data/data.js";

import HeroImage from "@/components/portfolio/Hero-Image.vue";
import HeroText from "@/components/portfolio/Hero-Text.vue";
import DotMenu from "@/components/portfolio/Dot-Menu.vue";
import Dot from "@/components/portfolio/Dot.vue";

export default {
	name: "Portfolio",
	components: {
		HeroImage,
		HeroText,
		DotMenu,
		Dot
	},
	data: function() {
		return {
			projects: DATA.projects,
			selected: null,
			choosing: false,
		  	centerOffset: 0,
		  	heroRotation: 0,
		  	menuActive: false,
		  	needsUpdating: false,
		  	instruct: true,
		  	instructions: '',
		  	margin: 7.5,
		}
	},
	methods: {
		updateOffsets() {
			let w = 0; /* Defualt */
			if(this.$refs.herotext){ /*Check if exist*/

				let vw = this.$refs.main.clientWidth;   /* view width */
				for (var i = 1 ; i < this.projects.length; i++) {   /* start on 1 */
					 /* prev section width
						$el is needed becuase component */			
					let pw = this.$refs.herotext[i-1].$el.clientWidth; 	
					w += (pw / vw) * 100; 
					w += this.margin;
					this.projects[i].offset = w ;				
				}
				// center 
				if (this.selected != null){
					let sw = this.$refs.herotext[this.selected].$el.clientWidth; 
					this.centerOffset = (vw/2) - (sw/2);
					this.centerOffset = (this.centerOffset / vw) * 100; /*Conver to %*/
				}	
			}
    	},
		translateOffset(id) {
    		let x = this.projects[id].offset;
    		let y = 0;
    		if (this.selected){
    			x -= this.projects[this.selected].offset; 
	    	}
	    	x += this.centerOffset;
    		return {transform: 'translate(' + x + 'vw, '+ y +'vh)'};
    	},
    	getInstructions() {
    		if (window.innerWidth > 750) {
				this.instructions = 'Hover over the dots to select a project'
			}
			else {
				this.instructions = 'Press & hold the dots to select a project.'
			}
			return this.instructions;
    	}
	},
	computed:{
		getSelectedImage() {
			if(this.selected != null){
				return this.projects[this.selected].img;
			}
		},
	},
	watch: {
		selected() {
			this.needsUpdating = true;
		},
		needsUpdating() {
			/* using request animation frame to get correct dom sizes (EVENT LOOP BIZ) */
			requestAnimationFrame(this.updateOffsets);
			/* change needs updating to false to run again and correctly update offsets*/
			this.needsUpdating = false;
			// console.log("updated-selecting");
		},
		choosing() {
			this.instruct = false;
			requestAnimationFrame(this.updateOffsets);
			// console.log("updated-choosing");
		}
	},
	created() {
    	window.addEventListener('resize', this.getInstructions);
    	this.getInstructions();
  	},
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

	.info-crop{
		width: 100vw;
		height: 50vh;
		overflow: hidden;
		position: absolute;
	}

	p.instructions{
		position: absolute;
	    bottom: 10vh;
	    text-align: center;
	    width: 100vw;
	    color: #818181;
	}
</style>