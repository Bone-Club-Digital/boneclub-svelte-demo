<script>
	import { onMount } from 'svelte';

	const panels = [
		{
			title: 'Play',
			text: 'Enter live matches, private rooms and club tables for elegant competitive play.'
		},
		{
			title: 'Learn',
			text: 'Sharpen your game with lessons, strategy, analysis and expert guidance.'
		},
		{
			title: 'Watch',
			text: 'Enjoy featured matches, streams and cinematic backgammon content from the club.'
		},
		{
			title: 'Social',
			text: 'Meet members, join conversations and experience the atmosphere beyond the board.'
		},
		{
			title: 'Shop',
			text: 'Explore boards, club goods and carefully curated Bone Club pieces.'
		}
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

		return () => {
			mq.removeEventListener('change', update);
		};
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
					console.log('Interior video play was blocked or not ready yet.', e);
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

<div
	class="scene"
	class:entering={entering}
	class:logging-in={loggingIn}
	class:interior-visible={interiorVideoVisible}
	class:show-panels={showFloatingPanels}
>
	<div
		class="camera"
		style="
			--zoom-scale: {zoomScale};
			--zoom-x: {zoomX}%;
			--zoom-y: {zoomY}%;
			transform: {cameraTransform};
		"
	>
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
			<video
				class="steam"
				class:safari={isSafari}
				autoplay
				muted
				loop
				playsinline
				preload="auto"
				webkit-playsinline="true"
			>
				{#if isSafari}
					<source src="https://www.boneclub.co.uk/steam2.mov" type='video/mp4; codecs="hvc1"' />
				{:else}
					<source src="https://www.boneclub.co.uk/smoke.webm" type="video/webm" />
				{/if}
			</video>
		</div>
	</div>

	<div class="interior-video-layer" class:visible={interiorVideoVisible}>
		<video
			bind:this={interiorVideoEl}
			class="interior-video"
			playsinline
			preload="auto"
			onloadeddata={handleInteriorReady}
			oncanplaythrough={handleInteriorReady}
			onended={handleInteriorEnded}
		>
			<source
				src="https://www.boneclub.co.uk/have_bones_character_welcome_y_Kling_30__45175.mp4"
				type="video/mp4"
			/>
		</video>

		<div class="floating-panels" class:visible={showFloatingPanels}>
			{#each panels as panel, index}
				<div class="floating-panel" style={`transition-delay: ${index * 0.35}s;`}>
					<h3>{panel.title}</h3>
					<p>{panel.text}</p>
				</div>
			{/each}
		</div>
	</div>

	<div
		class="enter-wrap"
		class:fading={buttonFading}
		style="
			left: {buttonLeft}%;
			top: {buttonTop}%;
			transform: translate({buttonTranslateX}%, {buttonTranslateY}%);
		"
	>
		<button class="fancy-btn enter-btn" type="button" onclick={handleEnter}>
			<span>
				<span class="spark"></span>
			</span>
			<span class="backdrop"></span>
			<span class="label">Enter Bone Club</span>
		</button>
	</div>

	<div class="modal-layer" class:visible={showLoginModal}>
		<div class="modal-dim" class:visible={showLoginModal}></div>

		<div class="modal-card" class:visible={showLoginModal}>
			<div class="modal-topline"></div>

			<div class="modal-header">
				<h2>Member Login</h2>
				<p>Enter the club</p>
			</div>

			<div class="modal-body">
				<label class="field">
					<span>Email</span>
					<input type="email" placeholder="member@boneclub.com" />
				</label>

				<label class="field">
					<span>Password</span>
					<input type="password" placeholder="Enter your password" />
				</label>

				<div class="modal-actions">
					<button class="modal-btn primary" type="button" onclick={handleLogin}>Login</button>
					<button class="modal-btn secondary" type="button">Request invitation</button>
				</div>
			</div>
		</div>
	</div>

	<div class="final-fade" class:visible={loggingIn}></div>
</div>

<style>
	:global(html, body) {
		margin: 0;
		padding: 0;
		width: 100%;
		height: 100%;
		background: #000;
		overflow: hidden;
	}

	:global(body) {
		touch-action: manipulation;
		overscroll-behavior: none;
	}

	:global(#app) {
		width: 100%;
		height: 100%;
	}

	:root {
		--bc-off: #e16a38;
		--bc-hover: #377e7f;
		--bc-bone: #f9f8ef;
		--bc-panel: rgba(14, 14, 14, 0);
		--bc-panel-border: rgba(249, 248, 239, 0.48);
	}

	.scene {
		position: fixed;
		inset: 0;
		width: 100vw;
		height: 100vh;
		max-width: none;
		aspect-ratio: auto;
		overflow: hidden;
		background-color: #111;
		background-image: url('https://www.boneclub.co.uk/exterior.jpg');
		background-size: cover;
		background-position: center;
		isolation: auto;
	}

	.camera {
		position: absolute;
		inset: 0;
		transform-origin: center center;
		will-change: transform;
		transition: transform 4.8s cubic-bezier(0.42, 0, 0.18, 1);
		z-index: 1;
	}

	.scene.logging-in .camera {
		transition: transform 3.2s cubic-bezier(0.2, 0.02, 0.12, 1);
	}

	.bg {
		position: absolute;
		inset: 0;
		background-image: url('https://www.boneclub.co.uk/exterior.jpg');
		background-size: cover;
		background-position: center;
	}

	.steam-wrap {
		position: absolute;
		pointer-events: none;
		z-index: 2;
		aspect-ratio: 920 / 320;
	}

	.steam {
		display: block;
		width: 100%;
		height: 100%;
		object-fit: contain;
		opacity: 0.45;
	}

	.steam.safari {
		mix-blend-mode: screen;
		opacity: 1;
	}

	.interior-video-layer {
		position: absolute;
		inset: 0;
		z-index: 2;
		opacity: 0;
		pointer-events: none;
		background: #000;
		transition: opacity 1.6s ease;
	}

	.interior-video-layer.visible {
		opacity: 1;
		pointer-events: auto;
	}

	.interior-video {
		width: 100%;
		height: 100%;
		object-fit: cover;
		display: block;
		background: #000;
	}

	.floating-panels {
		position: absolute;
		inset: 0;
		z-index: 4;
		display: grid;
		grid-template-columns: repeat(5, minmax(0, 1fr));
		gap: 16px;
		align-items: end;
		padding: 28px;
		pointer-events: none;
	}

	.floating-panel {
		align-self: end;
		padding: 18px 18px 16px;
		border-radius: 18px;
		background: rgba(10, 10, 10, 0.42);
		border: 1px solid rgba(249, 248, 239, 0.24);
		box-shadow:
			0 20px 40px rgba(0, 0, 0, 0.28),
			inset 0 1px 0 rgba(249, 248, 239, 0.06);
		backdrop-filter: blur(10px);
		-webkit-backdrop-filter: blur(10px);
		opacity: 0;
		transform: translateY(26px);
		transition:
			opacity 0.9s ease,
			transform 0.9s ease;
	}

	.floating-panels.visible .floating-panel {
		opacity: 1;
		transform: translateY(0);
	}

	.floating-panel h3 {
		margin: 0 0 8px;
		font-size: 18px;
		font-weight: 600;
		letter-spacing: 0.02em;
		color: var(--bc-bone);
	}

	.floating-panel p {
		margin: 0;
		font-size: 13px;
		line-height: 1.45;
		color: rgba(249, 248, 239, 0.82);
	}

	.enter-wrap {
		position: absolute;
		z-index: 3;
		pointer-events: auto;
		opacity: 1;
		transition: opacity 2.2s ease;
	}

	.enter-wrap.fading {
		opacity: 0;
	}

	.enter-btn {
		pointer-events: auto;
		position: relative;
		z-index: 3;
	}

	.fancy-btn {
		position: relative;
		display: grid;
		overflow: hidden;
		border-radius: 14px;
		padding: 12px 22px;
		cursor: pointer;
		background: transparent;
		border: none;
		font-size: 15px;
		letter-spacing: 0.04em;
		box-shadow: inset 0 1000px 0 0 var(--bc-off);
		transition: box-shadow 0.45s ease;
	}

	.spark {
		position: absolute;
		inset: 0;
		width: 100%;
		height: 100%;
		overflow: hidden;
		border-radius: 14px;
		-webkit-mask: linear-gradient(white, transparent 50%);
		mask: linear-gradient(white, transparent 50%);
		animation: flip 6s infinite steps(2, end);
	}

	.spark::before {
		content: '';
		position: absolute;
		inset: 0 auto auto 50%;
		width: 200%;
		aspect-ratio: 1;
		transform: translate(-50%, -15%) rotate(-90deg);
		background: conic-gradient(from 0deg, transparent 0 340deg, var(--bc-bone) 360deg);
		animation: kitrotate 3s linear infinite both;
	}

	.backdrop {
		position: absolute;
		inset: 1px;
		border-radius: 13px;
		background: var(--bc-off);
		transition: background 0.45s ease;
	}

	.label {
		position: relative;
		z-index: 2;
		color: var(--bc-bone);
		font-weight: 500;
		transition: none;
	}

	.enter-btn:hover {
		box-shadow: inset 0 1000px 0 0 var(--bc-hover);
		filter: none;
	}

	.enter-btn:hover .backdrop {
		background: var(--bc-hover);
	}

	.enter-btn:hover .label {
		color: var(--bc-bone);
	}

	.modal-layer {
		position: absolute;
		inset: 0;
		z-index: 4;
		display: grid;
		place-items: center;
		padding: 24px;
		pointer-events: none;
	}

	.modal-layer.visible {
		pointer-events: auto;
	}

	.modal-dim {
		position: absolute;
		inset: 0;
		background: #000;
		opacity: 0;
		transition: opacity 1.4s ease;
		z-index: 0;
	}

	.modal-dim.visible {
		opacity: 0.5;
	}

	.modal-card {
		position: relative;
		z-index: 1;
		width: min(420px, 92vw);
		border-radius: 18px;
		background: var(--bc-panel);
		border: 0 solid var(--bc-panel-border);
		box-shadow:
			0 24px 80px rgba(0, 0, 0, 0.05),
			inset 0 1px 0 rgba(249, 248, 239, 0.06);
		backdrop-filter: blur(0px);
		-webkit-backdrop-filter: blur(0px);
		overflow: hidden;
		transform: translateY(24px) scale(0.98);
		transition:
			transform 1.4s ease,
			opacity 1.4s ease;
		opacity: 0;
	}

	.modal-card.visible {
		transform: translateY(0) scale(1);
		opacity: 1;
	}

	.scene.logging-in .modal-card {
		transform: translateY(-8px) scale(0.96);
		opacity: 0;
		transition:
			transform 0.8s ease,
			opacity 0.8s ease;
	}

	.scene.logging-in .modal-dim {
		opacity: 0;
		transition: opacity 0.8s ease;
	}

	.modal-topline {
		height: 0px;
		background: linear-gradient(90deg, transparent, var(--bc-bone), transparent);
		opacity: 0.8;
	}

	.modal-header {
		padding: 24px 24px 10px;
	}

	.modal-header h2 {
		margin: 0;
		font-size: 24px;
		font-weight: 600;
		letter-spacing: 0.02em;
		color: var(--bc-bone);
	}

	.modal-header p {
		margin: 6px 0 0;
		color: rgba(249, 248, 239, 0.7);
		font-size: 14px;
	}

	.modal-body {
		padding: 12px 24px 24px;
		display: grid;
		gap: 14px;
	}

	.field {
		display: grid;
		gap: 6px;
	}

	.field span {
		font-size: 13px;
		color: rgba(249, 248, 239, 0.84);
	}

	.field input {
		width: 100%;
		padding: 12px 14px;
		border-radius: 12px;
		border: 1px solid rgba(249, 248, 239, 0.74);
		background: rgba(0, 0, 0, 0.4);
		color: var(--bc-bone);
		outline: none;
		font: inherit;
		box-sizing: border-box;
	}

	.field input::placeholder {
		color: rgba(249, 248, 239, 0.75);
	}

	.field input:focus {
		border-color: rgba(249, 248, 239, 0.35);
		box-shadow: 0 0 0 3px rgba(249, 248, 239, 0.06);
	}

	.modal-actions {
		display: grid;
		gap: 10px;
		margin-top: 6px;
	}

	.modal-btn {
		padding: 12px 14px;
		border-radius: 12px;
		border: 0;
		font: inherit;
		cursor: pointer;
		transition:
			background 0.35s ease,
			color 0.35s ease,
			border-color 0.35s ease,
			opacity 0.35s ease;
	}

	.modal-btn.primary {
		background: var(--bc-off);
		color: var(--bc-bone);
	}

	.modal-btn.primary:hover {
		background: var(--bc-hover);
	}

	.modal-btn.secondary {
		background: transparent;
		color: var(--bc-bone);
		border: 1px solid rgba(249, 248, 239, 0.18);
	}

	.modal-btn.secondary:hover {
		background: rgba(249, 248, 239, 0.06);
	}

	.final-fade {
		position: absolute;
		inset: 0;
		z-index: 6;
		background: #000;
		opacity: 0;
		pointer-events: none;
		transition: opacity 2.2s ease;
	}

	.final-fade.visible {
		opacity: 1;
	}

	.scene.interior-visible .final-fade {
		opacity: 0;
		transition: opacity 1.8s ease;
	}

	@media (max-width: 900px) {
		.floating-panels {
			grid-template-columns: repeat(2, minmax(0, 1fr));
			align-content: end;
		}
	}

	@media (max-width: 640px) {
		.steam {
			opacity: 0.38;
		}

		.steam.safari {
			opacity: 0.95;
		}

		.modal-header {
			padding: 20px 20px 8px;
		}

		.modal-body {
			padding: 10px 20px 20px;
		}

		.floating-panels {
			grid-template-columns: 1fr;
			padding: 16px;
			gap: 10px;
		}

		.floating-panel {
			padding: 14px 14px 13px;
			border-radius: 14px;
		}

		.floating-panel h3 {
			font-size: 16px;
		}

		.floating-panel p {
			font-size: 12px;
		}
	}

	@keyframes flip {
		to {
			transform: rotate(360deg);
		}
	}

	@keyframes kitrotate {
		to {
			transform: translate(-50%, -15%) rotate(0deg);
		}
	}
</style>
