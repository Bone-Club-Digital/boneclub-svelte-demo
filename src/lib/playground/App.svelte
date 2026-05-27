<script>
	import { onMount } from 'svelte';

	const ua = navigator.userAgent;
	const isSafari =
		/safari/i.test(ua) &&
		!/chrome|android|crios|fxios|edgios/i.test(ua);

	let isMobile = false;
	let accordionOpen = false;
	let entering = false;
	let buttonFading = false;
	let showLoginModal = false;
	let loggingIn = false;

	let interiorVideoVisible = false;
	let interiorVideoReady = false;
	let interiorVideoEl;

	let steamLeft = 78;
	let steamTop = 68;
	let steamWidth = 100;
	let steamTranslateX = -50;
	let steamTranslateY = -50;

	let steamLeftMobile = 83.5;
	let steamTopMobile = 76.5;
	let steamWidthMobile = 84;
	let steamTranslateXMobile = -50;
	let steamTranslateYMobile = -50;

	let buttonLeft = 17.5;
	let buttonTop = 88;
	let buttonTranslateX = -50;
	let buttonTranslateY = -50;

	let zoomScale = 2.55;
	let zoomX = 23;
	let zoomY = -34;

	let loginZoomScale = 3.7;
	let loginZoomX = 38;
	let loginZoomY = -52;

	const zoomDuration = 4800;
	const modalLeadTime = 3000;
	const blackCoverDuration = 2200;

	onMount(() => {
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

	$: cameraTransform = loggingIn
		? `translate(${loginZoomX}%, ${loginZoomY}%) scale(${loginZoomScale})`
		: entering
			? `translate(${zoomX}%, ${zoomY}%) scale(${zoomScale})`
			: 'translate(0%, 0%) scale(1)';
</script>

<div class="controls">
	<button
		class="accordion-toggle"
		type="button"
		on:click={() => (accordionOpen = !accordionOpen)}
		aria-expanded={accordionOpen}
	>
		<span>Steam + button + camera controls</span>
		<span class:open={accordionOpen} class="chevron">▼</span>
	</button>

	{#if accordionOpen}
		<div class="accordion-panel">
			<h3>Desktop</h3>

			<div class="control">
				<label>Left: {steamLeft}%</label>
				<input type="range" min="-50" max="100" step="0.5" bind:value={steamLeft} />
			</div>

			<div class="control">
				<label>Top: {steamTop}%</label>
				<input type="range" min="-50" max="100" step="0.5" bind:value={steamTop} />
			</div>

			<div class="control">
				<label>Width: {steamWidth}vw</label>
				<input type="range" min="10" max="120" step="1" bind:value={steamWidth} />
			</div>

			<div class="control">
				<label>Translate X: {steamTranslateX}%</label>
				<input type="range" min="-100" max="100" step="1" bind:value={steamTranslateX} />
			</div>

			<div class="control">
				<label>Translate Y: {steamTranslateY}%</label>
				<input type="range" min="-100" max="100" step="1" bind:value={steamTranslateY} />
			</div>

			<h3>Mobile</h3>

			<div class="control">
				<label>Left: {steamLeftMobile}%</label>
				<input type="range" min="-50" max="100" step="0.5" bind:value={steamLeftMobile} />
			</div>

			<div class="control">
				<label>Top: {steamTopMobile}%</label>
				<input type="range" min="-50" max="100" step="0.5" bind:value={steamTopMobile} />
			</div>

			<div class="control">
				<label>Width: {steamWidthMobile}vw</label>
				<input type="range" min="10" max="150" step="1" bind:value={steamWidthMobile} />
			</div>

			<div class="control">
				<label>Translate X: {steamTranslateXMobile}%</label>
				<input type="range" min="-100" max="100" step="1" bind:value={steamTranslateXMobile} />
			</div>

			<div class="control">
				<label>Translate Y: {steamTranslateYMobile}%</label>
				<input type="range" min="-100" max="100" step="1" bind:value={steamTranslateYMobile} />
			</div>

			<h3>Button</h3>

			<div class="control">
				<label>Left: {buttonLeft}%</label>
				<input type="range" min="0" max="100" step="0.5" bind:value={buttonLeft} />
			</div>

			<div class="control">
				<label>Top: {buttonTop}%</label>
				<input type="range" min="0" max="100" step="0.5" bind:value={buttonTop} />
			</div>

			<div class="control">
				<label>Translate X: {buttonTranslateX}%</label>
				<input type="range" min="-100" max="100" step="1" bind:value={buttonTranslateX} />
			</div>

			<div class="control">
				<label>Translate Y: {buttonTranslateY}%</label>
				<input type="range" min="-100" max="100" step="1" bind:value={buttonTranslateY} />
			</div>

			<h3>Camera</h3>

			<div class="control">
				<label>Zoom scale: {zoomScale}</label>
				<input type="range" min="1" max="4" step="0.01" bind:value={zoomScale} />
			</div>

			<div class="control">
				<label>Zoom X: {zoomX}%</label>
				<input type="range" min="-50" max="50" step="0.5" bind:value={zoomX} />
			</div>

			<div class="control">
				<label>Zoom Y: {zoomY}%</label>
				<input type="range" min="-50" max="50" step="0.5" bind:value={zoomY} />
			</div>

			<h3>Login zoom</h3>

			<div class="control">
				<label>Login zoom scale: {loginZoomScale}</label>
				<input type="range" min="1" max="6" step="0.01" bind:value={loginZoomScale} />
			</div>

			<div class="control">
				<label>Login zoom X: {loginZoomX}%</label>
				<input type="range" min="-50" max="50" step="0.5" bind:value={loginZoomX} />
			</div>

			<div class="control">
				<label>Login zoom Y: {loginZoomY}%</label>
				<input type="range" min="-50" max="50" step="0.5" bind:value={loginZoomY} />
			</div>

			<p>Currently using: <strong>{isMobile ? 'mobile' : 'desktop'}</strong> values</p>
			<p>Interior video ready: <strong>{interiorVideoReady ? 'yes' : 'loading'}</strong></p>
		</div>
	{/if}
</div>

<div
	class="scene"
	class:entering={entering}
	class:logging-in={loggingIn}
	class:interior-visible={interiorVideoVisible}
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
		<source src="https://video.wixstatic.com/video/f92ddd_ee6ec0b4d5fc431ba40a1fd122fedb03/1080p/mp4/file.mp4" type='video/mp4; codecs="hvc1"' />
	{:else}
		<source src="https://video.wixstatic.com/video/f92ddd_683efc8d19ff4a179bf528450e49be03/360p/mp4/file.mp4" type="video/webm" />
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
			on:loadeddata={handleInteriorReady}
			on:canplaythrough={handleInteriorReady}
		>
			<source
				src="https://video.wixstatic.com/video/f92ddd_6cd1eedb3a2d4ffeacd309bf30545c65/1080p/mp4/file.mp4"
				type="video/mp4"
			/>
		</video>
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
		<button class="fancy-btn enter-btn" type="button" on:click={handleEnter}>
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
					<button class="modal-btn primary" type="button" on:click={handleLogin}>Login</button>
					<button class="modal-btn secondary" type="button">Request invitation</button>
				</div>
			</div>
		</div>
	</div>

	<div class="final-fade" class:visible={loggingIn}></div>
</div>

<style>
	:root {
		--bc-off: #e16a38;
		--bc-hover: #377e7f;
		--bc-bone: #f9f8ef;
		--bc-panel: rgba(14, 14, 14, 0);
		--bc-panel-border: rgba(249, 248, 239, 0.48);
	}

	.controls {
		max-width: 1440px;
		margin: 0 auto 16px;
		background: #1a1a1a;
		color: white;
		font: 14px/1.4 Arial, sans-serif;
		border-radius: 10px;
		overflow: hidden;
	}

	.accordion-toggle {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 12px 14px;
		background: #1a1a1a;
		color: white;
		border: 0;
		cursor: pointer;
		font: inherit;
		text-align: left;
	}

	.accordion-panel {
		padding: 0 12px 12px;
		display: grid;
		gap: 10px;
		border-top: 1px solid rgba(255, 255, 255, 0.08);
	}

	.chevron {
		transition: transform 0.2s ease;
	}

	.chevron.open {
		transform: rotate(180deg);
	}

	.control {
		display: grid;
		gap: 4px;
	}

	.control input {
		width: 100%;
	}

	.scene {
		position: relative;
		width: 100%;
		max-width: 1440px;
		aspect-ratio: 16 / 9;
		overflow: hidden;
		background-color: #111;
		background-image: url('https://static.wixstatic.com/media/f92ddd_9ae8a2bb729647bca09862f9875cb6b4~mv2.jpg');
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
		background-image: url('https://static.wixstatic.com/media/f92ddd_9ae8a2bb729647bca09862f9875cb6b4~mv2.jpg');
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
	}

	.interior-video {
		width: 100%;
		height: 100%;
		object-fit: cover;
		display: block;
		background: #000;
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

	@media (max-width: 640px) {
		.scene {
			aspect-ratio: 16 / 11;
		}

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
