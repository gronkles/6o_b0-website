<svelte:head>
	<title>6o_b0 | Contact</title>
</svelte:head>

<h1>Contact</h1>

<div class="contact">
	<p>Contact me! email:</p>
	<p class="email"><a href="mailto:hello@johndoe.example">hello@johndoe.example</a></p>

	<h2>Elsewhere</h2>
	<ul>
		<li><a href="https://soundcloud.com/">SoundCloud</a></li>
		<li><a href="https://youtube.com/">YouTube</a></li>
		<li><a href="https://instagram.com/">Instagram</a></li>
	</ul>
</div>

<style>
	.contact {
		max-width: 38rem;
		margin-top: 2rem;
		font-size: 1.125rem;
	}

	.email {
		font-size: clamp(1.25rem, 3vw, 1.75rem);
		margin-top: 0;
	}

	a {
		color: var(--accent);
		text-underline-offset: 0.2em;
	}

	h2 {
		font-size: 1.9rem;
		font-weight: 500;
		margin-top: 3rem;
	}

	ul {
		list-style: none;
		padding: 0;
		margin: 1rem 0 0;
		display: grid;
		gap: 0.5rem;
	}
</style>
