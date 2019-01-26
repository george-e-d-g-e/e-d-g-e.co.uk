<template>
	<section class="infomation">
		<h2> {{ project.title }} </h2>
		<p class="tagline"> {{ project.tagline }} </p>
		<p class="meta"> {{ project.meta }} </p>
		
		<div 
			class="cta"
			:href="'/projects' + project.url"
		> View </div>
	</section>
</template>

<script>
	export default {
  		name: "HeroText",
  		props: {
  			project: Object
  		}
	};
</script>

<style scoped lang="scss">
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
			font-weight: normal;
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
			transition: 
				opacity 0.5s ease-in-out 0.2s, 
				transform 0.5s ease-in-out 0.2s, 
				max-height 0.5s ease-in-out 0.0s, 
				margin 0.5s ease-in-out 0.0s;
		}

		.meta {
			font-size: 0.8em;
			transition: 
			transform 0.5s ease-in-out 0.2s, 
		}

		.cta {
			opacity: 0;
			display: none;
			width: auto;
			cursor: pointer;
			border: 0.05em solid black;
			border-radius: 0.1em;
			padding: 0.5em 1em;
			margin:  1em auto 0 auto;
			transition: opacity 0.5s;
		}

		.cta:hover {
			background: black;
			color: white;
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
		.cta {
			opacity: 1;
			display: inline-block;
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
		.cta {
			opacity: 0;

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

	

</style>