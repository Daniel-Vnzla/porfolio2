<script>
	import { onDestroy } from 'svelte';
	import Line from '../../components/common/Line.svelte';
	import Marco from '../common/Marco.svelte';
	import AboutMe from './AboutMe.svelte';

	export let active;
	export let animationDelay = 0;
	let animationDone = false;

	const lazyLodingImg = ({ target }) => {
		target.style.transform = "translateX(0)"
		target.style.opacity = 1
	};

	const introAnimationDone = setTimeout(()=>{
		animationDone = true;
	}, animationDelay);

	onDestroy(() => clearTimeout(introAnimationDone));
</script>
 
{#if animationDone}
	<section class="about">
		<div class="about-wrapper">
			<div class="perfil">
				<Marco color="var(--about-color)"/>
				<img 
					class="img" 
					on:load={lazyLodingImg} 
					src="/images/me.jpg" 
					alt="Yordinson Polar">
			</div>
			<span class="line">
				<Line
				  time="var(--transition-time)"
				  color="var(--about-color)" 
					distance="100%" 
					position="bottom" 
					origin="left" 
					volumen="4px" delay="calc(1.2 * var(--transition-time))" />
			</span>
			<AboutMe bind:active />
		</div>
	</section>
{/if}



<style>
	.about {
		position: relative;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
		padding-bottom: 40px;
	}

	.about-wrapper {
		width: 90%;
		display: grid;
		grid-template-columns: 4fr 1fr 15fr;
		align-items: center;
	}

	.perfil {
		position: relative;
		height: 80%;
		width: 100%;
	}

	.img {
		opacity: 0;
		position: relative;
		width: 100%;
		height: 100%;
		transform: translateX(-10%); 
		transition: 
			opacity var(--transition-time),
			transform var(--transition-time);
		transition-delay: 1200ms; 
	}

	.line {
		position: relative;
		height: 4px;
	}

	@media (max-width: 760px) {
		.about {
			height: 100%;
			padding-bottom: 0;
		}

		.about-wrapper {
			width: 80%;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			padding: 0 20px;
		}

		.img {
			min-height: 300px;
		}

	}
	
</style>