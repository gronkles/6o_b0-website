<script>
	import { resolve } from '$app/paths';
	import { tick } from 'svelte';
	import { page } from '$app/state';
	import Curtain from '$lib/Curtain.svelte';

	let { children } = $props();

	const links = [
		{ href: '/', label: 'Projects' },
		{ href: '/about', label: 'About' },
		{ href: '/contact', label: 'Contact' }
	];

	const isHome = $derived(page.url.pathname === '/');

	// The curtain only plays when a visit starts on the home page.
	// The layout stays mounted while navigating, so this remembers it has already opened.
	let introOpen = $state(page.url.pathname !== '/');
	let mainEl;

	async function handleOpened() {
		introOpen = true;
		await tick();
		mainEl?.focus(); // move keyboard focus to the site once the curtain is gone
	}
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Big+Shoulders+Display:wght@500;800&family=Instrument+Sans:wght@400;600&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

{#if isHome && !introOpen}
	<Curtain first="6o" last="b0" onopened={handleOpened} />
{/if}

<!-- Everything behind the curtain is unreachable by keyboard until it opens -->
<div class="site" inert={isHome && !introOpen}>
	<header class="site-header">
		<a class="name" href={resolve('/')}>6o_b0</a>

		<nav aria-label="Main">
			<ul>
				{#each links as link (link.href)}
					<li>
						<a
							href={resolve(link.href)}
							aria-current={page.url.pathname === resolve(link.href) ? 'page' : undefined}
						>
							{link.label}
						</a>
					</li>
				{/each}
			</ul>
		</nav>
	</header>

	<main bind:this={mainEl} tabindex="-1">
		{@render children()}
	</main>
</div>

<style>
	:global(:root) {
		--bg: #2e2e2e;
		--ink: #ecebf3;
		--muted: #5b5775;
		--accent: #d3965d;
		--placeholder: #d8d6e8;
		--pad: clamp(1.5rem, 5vw, 4rem);
	}

	:global(body) {
		margin: 0;
		background: var(--bg);
		color: var(--ink);
		font-family: 'Instrument Sans', system-ui, sans-serif;
		line-height: 1.5;
	}

	:global(h1),
	:global(h2) {
		font-family: 'Big Shoulders Display', 'Arial Narrow', sans-serif;
		line-height: 0.9;
		margin: 0;
	}

	:global(h1) {
		font-size: clamp(3.5rem, 10vw, 7rem);
		font-weight: 800;
		color: var(--accent);
	}

	.site-header,
	main {
		max-width: 1200px;
		margin: 0 auto;
		padding-inline: var(--pad);
	}

	.site-header {
		display: flex;
		align-items: center;
		gap: 1rem;
		padding-block: 1.5rem;
	}

	.name {
		font-family: 'Big Shoulders Display', 'Arial Narrow', sans-serif;
		font-weight: 800;
		font-size: 1.75rem;
		line-height: 1;
		color: var(--accent);
		text-decoration: none;
	}

	nav {
		margin-left: auto; /* keeps the links on the right even when the name is hidden */
	}

	nav ul {
		display: flex;
		gap: clamp(1rem, 3vw, 2rem);
		margin: 0;
		padding: 0;
		list-style: none;
	}

	nav a {
		color: var(--ink);
		text-decoration: none;
		text-underline-offset: 0.3em;
		text-decoration-thickness: 2px;
	}

	nav a:hover,
	nav a[aria-current='page'] {
		text-decoration-line: underline;
	}

	nav a[aria-current='page'] {
		color: var(--accent);
	}

	main {
		padding-top: clamp(1rem, 4vw, 3rem);
		padding-bottom: var(--pad);
	}

	main:focus {
		outline: none; /* focus is moved here by script, not by the user */
	}
</style>
