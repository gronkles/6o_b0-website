<script>
	let { first = '6o', last = 'b0', onopened } = $props();

	let opening = $state(false);
	let done = $state(false);

	function finish() {
		done = true;
		onopened?.();
	}

	function open() {
		if (opening) return;
		opening = true;

		// With reduced motion there's no slide, so skip straight to the site
		if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) finish();
	}

	function handleTransitionEnd(event) {
		// The left panel finishing its slide means the curtain is fully open
		if (event.target.classList.contains('left') && event.propertyName === 'transform') {
			finish();
		}
	}

	// Stop the page scrolling behind the curtain while it's closed
	$effect(() => {
		document.body.style.overflow = done ? '' : 'hidden';
		return () => (document.body.style.overflow = '');
	});
</script>

{#if !done}
	<div class="curtain" class:opening ontransitionend={handleTransitionEnd}>
		<div class="panel left"><span>{first}</span></div>
		<div class="panel right"><span>{last}</span></div>

		<button class="play" onclick={open} aria-label="Enter site">
			<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M8 5.5v13l11-6.5z" /></svg>
		</button>
	</div>
{/if}

<style>
	.curtain {
		--curtain: #2e2e2e;
		--light: #ecebf3;
		--btn: clamp(4.5rem, 9vw, 7rem);
		--gap: clamp(0.75rem, 2vw, 1.75rem);
		--ease: cubic-bezier(0.77, 0, 0.18, 1);

		position: fixed;
		inset: 0;
		z-index: 100;
		display: grid;
		grid-template-columns: 1fr 1fr;
	}

	.panel {
		display: flex;
		align-items: center;
		background: var(--curtain);
		color: var(--light);
		font-family: 'Big Shoulders Display', 'Arial Narrow', 'Helvetica Neue', sans-serif;
		font-weight: 800;
		font-size: clamp(3.5rem, 16vw, 15rem);
		line-height: 0.85;
		letter-spacing: -0.01em;
		transition: transform 1.2s var(--ease) 0.2s;
	}

	.left {
		justify-content: flex-end;
		padding-right: calc(var(--btn) / 2 + var(--gap));
		margin-right: -1px; /* hides any hairline gap at the seam */
	}

	.right {
		justify-content: flex-start;
		padding-left: calc(var(--btn) / 2 + var(--gap));
	}

	.opening .left {
		transform: translateX(-100%);
	}

	.opening .right {
		transform: translateX(100%);
	}

	.play {
		position: absolute;
		top: 50%;
		left: 50%;
		translate: -50% -50%;
		width: var(--btn);
		height: var(--btn);
		display: grid;
		place-items: center;
		border: 0;
		border-radius: 50%; /* no longer visible, but keeps the focus outline round */
		background: none;
		color: var(--light);
		cursor: pointer;
		transition:
			scale 0.3s var(--ease),
			opacity 0.3s ease;
	}

	.play svg {
		width: 80%;
		margin-left: 8%;
		fill: currentColor;
	}

	.play:hover {
		scale: 1.06;
	}

	.play:focus-visible {
		outline: 3px solid var(--light);
		outline-offset: 5px;
	}

	.opening .play {
		scale: 0.6;
		opacity: 0;
		pointer-events: none;
	}

	@media (prefers-reduced-motion: reduce) {
		.panel,
		.play {
			transition: none;
		}
	}
</style>
