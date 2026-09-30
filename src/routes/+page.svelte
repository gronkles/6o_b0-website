<script>
	// Placeholder content for the mockup. Later this comes from items.json via the CMS.
	const items = [
		{
			title: 'Placeholder single 1',
			type: 'Single',
			year: 2025,
			description: 'item description...'
		},
		{ title: 'Placeholder album', type: 'Album', year: 2024, description: 'item description...' },
		{ title: 'Placeholder EP', type: 'EP', year: 2024, description: 'item description...' },
		{
			title: 'Placeholder music video',
			type: 'Video',
			year: 2023,
			description: 'item description...'
		},
		{
			title: 'Placeholder single 2',
			type: 'Single',
			year: 2023,
			description: 'item description...'
		},
		{ title: 'Placeholder album ', type: 'Album', year: 2022, description: 'item description...' }
	];
</script>

<svelte:head>
	<title>6o_b0 | Projects</title>
</svelte:head>

<section class="grid" aria-label="Projects">
	{#each items as item (item.title)}
		<article class="tile">
			<div class="player">Player embed</div>
			<h2>{item.title}</h2>
			<p class="meta">{item.type}, {item.year}</p>
			<p>{item.description}</p>
		</article>
	{/each}
</section>

<style>
	.intro {
		margin-bottom: clamp(2.5rem, 6vw, 5rem);
		max-width: 40rem;
	}

	.intro p {
		font-size: 1.125rem;
		margin: 1rem 0 0;
	}

	.grid {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(min(100%, 280px), 1fr));
		gap: 3rem 2rem;
	}

	.player {
		aspect-ratio: 16 / 9;
		display: grid;
		place-items: center;
		background: var(--placeholder);
		color: var(--muted);
		font-size: 0.875rem;
		margin-bottom: 1rem;
	}

	h2 {
		font-size: 1.9rem;
		font-weight: 500;
	}

	.meta {
		color: var(--muted);
		margin: 0.25rem 0 0.5rem;
	}

	.tile p:last-child {
		margin: 0;
		max-width: 34ch;
	}
</style>
