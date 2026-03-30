<script>
	import { onMount } from 'svelte';

	const panels = [
		{ title: 'Play', text: 'Enter live matches, private rooms and club tables for elegant competitive play.' },
		{ title: 'Learn', text: 'Sharpen your game with lessons, strategy, analysis and expert guidance.' },
		{ title: 'Watch', text: 'Enjoy featured matches, streams and cinematic backgammon content from the club.' },
		{ title: 'Social', text: 'Meet members, join conversations and experience the atmosphere beyond the board.' },
		{ title: 'Shop', text: 'Explore boards, club goods and carefully curated Bone Club pieces.' }
	];

	let isSafari = $state(false);
	let isMobile = $state(false);
	let entering = $state(false);
	let buttonFading = $state(false);
	let showLoginModal = $state(false);
	let loggingIn = $state(false);

	let interiorVideoVisible = $state(false);
	let interiorVideoReady = $state(false);
	let interiorVideoEl = $state();
	let showFloatingPanels = $state(false);

	let steamLeft = $state(78);
	let steamTop = $state(68);
	let steamWidth = $state(100);
	let steamTranslateX = $state(-50);
	let steamTranslateY = $state(-50);

	let steamLeftMobile = $state(83.5);
	let steamTopMobile = $state(76.5);
	let steamWidthMobile = $state(84);
	let steamTranslateXMobile = $state(-50);
	let steamTranslateYMobile = $state(-50);

	let buttonLeft = $state(17.5);
	let buttonTop = $state(88);
	let buttonTranslateX = $state(-50);
	let buttonTranslateY = $state(-50);

	let zoomScale = $state(2.55);
	let zoomX = $state(23);
	let zoomY = $state(-34);

	let loginZoomScale = $state(3.7);
	let loginZoomX = $state(38);
	let loginZoomY = $state(-52);

	const zoomDuration = 4800;
	const modalLeadTime = 3000;
	const blackCoverDuration = 2200;

	const cameraTransform = $derived(
		loggingIn
			? `translate(${loginZoomX}%, ${loginZoomY}%) scale(${loginZoomScale})`
			: entering
				? `translate(${zoomX}%, ${zoomY}%) scale(${zoomScale})`
				: 'translate(0%, 0%) scale(1)'
	);

	onMount(() => {
		const ua = navigator.userAgent;
		isSafari =
			/safari/i.test(ua) &&
			!/chrome|android|crios|fxios|edgios/i.test(ua);

		const mq = window.matchMedia('(max-width: 640px)');

		const update = () => {
			isMobile = mq.matches;
		};

		update();
		mq.addEventListener('change', update);

		return () => mq.removeEventListener('change', update);
	});

	function handleEnter() {
		if (entering || buttonFading) return;
		buttonFading = true;
		entering = true;

		setTimeout(() => {
			showLoginModal = true;
		}, Math.max(0, zoomDuration - modalLeadTime));
	}

	async function handleLogin() {
		if (loggingIn) return;

		loggingIn = true;
		showLoginModal = false;
		interiorVideoVisible = false;
		showFloatingPanels = false;
		interiorVideoReady = false;

		setTimeout(async () => {
			interiorVideoVisible = true;

			if (interiorVideoEl) {
				try {
					interiorVideoEl.pause();
					interiorVideoEl.currentTime = 0;
					await interiorVideoEl.play();
				} catch (e) {
					console.log('Video blocked', e);
				}
			}
		}, blackCoverDuration);
	}

	function handleInteriorReady() {
		interiorVideoReady = true;
	}

	function handleInteriorEnded() {
		showFloatingPanels = true;
	}
</script>

<svelte:head>
	<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
</svelte:head>

<div class="scene">
	<div class="camera" style="transform: {cameraTransform};">
		<div class="bg"></div>

		<div
			class="steam-wrap"
			style="
				left: {isMobile ? steamLeftMobile : steamLeft}%;
				top: {isMobile ? steamTopMobile : steamTop}%;
				width: min(920px, {isMobile ? steamWidthMobile : steamWidth}vw);
				transform: translate({isMobile ? steamTranslateXMobile : steamTranslateX}%, {isMobile ? steamTranslateYMobile : steamTranslateY}%);
			"
		>
			<video class="steam" class:safari={isSafari} autoplay muted loop playsinline preload="auto">
				{#if isSafari}
					<source src="https://www.boneclub.co.uk/steam2.mov" type='video/mp4; codecs="hvc1"' />
				{:else}
					<source src="https://www.boneclub.co.uk/smoke.webm" type="video/webm" />
				{/if}
			</video>
		</div>
	</div>

	<div class="interior-video-layer" class:visible={interiorVideoVisible}>
		<video bind:this={interiorVideoEl} class="interior-video" playsinline preload="auto"
			onloadeddata={handleInteriorReady}
			oncanplaythrough={handleInteriorReady}
			onended={handleInteriorEnded}
		>
			<source src="https://www.boneclub.co.uk/have_bones_character_welcome_y_Kling_30__45175.mp4" />
		</video>

		<div class="floating-panels" class:visible={showFloatingPanels}>
			{#each panels as panel, i}
				<div class="floating-panel" style={`transition-delay:${i * 0.35}s`}>
					<h3>{panel.title}</h3>
					<p>{panel.text}</p>
				</div>
			{/each}
		</div>
	</div>

	<div class="enter-wrap" class:fading={buttonFading}
		style="left:{buttonLeft}%;top:{buttonTop}%;transform:translate({buttonTranslateX}%,{buttonTranslateY}%);">
		<button onclick={handleEnter} class="fancy-btn">
			<span class="label">Enter Bone Club</span>
		</button>
	</div>
</div>

<style>
:global(html, body) {
	margin: 0;
	padding: 0;
	height: 100%;
	overflow: hidden;
	background: #000;
}

/* SCENE */
.scene {
	position: fixed;
	inset: 0;
	width: 100vw;
	height: 100vh;
	height: 100dvh;
	overflow: hidden;
}

/* BACKGROUND */
.bg {
	position: absolute;
	inset: 0;
	background-image: url('https://www.boneclub.co.uk/exterior.jpg');
	background-size: cover;
	background-position: center;
}

/* MOBILE FIX */
@media (max-width: 640px) {
	.scene {
		height: 100svh;
		height: 100dvh;
	}

	.bg {
		background-size: auto 100%;
		background-position: center top;
	}
}

/* CAMERA */
.camera {
	position: absolute;
	inset: 0;
	transition: transform 4.8s ease;
}

/* VIDEO */
.interior-video-layer {
	position: absolute;
	inset: 0;
	opacity: 0;
	transition: opacity 1.6s;
}
.interior-video-layer.visible {
	opacity: 1;
}
.interior-video {
	width: 100%;
	height: 100%;
	object-fit: cover;
}

/* PANELS */
.floating-panels {
	position: absolute;
	inset: 0;
	display: grid;
	grid-template-columns: repeat(5, 1fr);
	gap: 16px;
	padding: 20px;
	align-items: end;
}
.floating-panel {
	background: rgba(0,0,0,0.4);
	padding: 16px;
	border-radius: 12px;
	opacity: 0;
	transform: translateY(20px);
	transition: 0.8s;
}
.floating-panels.visible .floating-panel {
	opacity: 1;
	transform: translateY(0);
}

/* BUTTON */
.enter-wrap {
	position: absolute;
}
.fancy-btn {
	background: #e16a38;
	color: #fff;
	padding: 12px 20px;
	border-radius: 10px;
	border: none;
	cursor: pointer;
}
</style>