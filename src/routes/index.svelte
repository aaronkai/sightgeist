<script>
	import Profile from '$lib/components/Profile.svelte';
	import { user } from '$lib/stores/sessionStore';
	import { supabase } from '$lib/supabaseClient';
	import '$lib/styles/reset.css';
	import '$lib/styles/global.css';
	import AuthForm from '$lib/components/AuthForm.svelte';

	let ctaVisible = true;

	//instantiate user store
	user.set(supabase.auth.user());

	//when there is a change to auth status, update the user store
	supabase.auth.onAuthStateChange((_, session) => {
		user.set(session.user);
	});

	function handleClick() {
		ctaVisible = !ctaVisible;
	}
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
	<link
		href="https://fonts.googleapis.com/css2?family=Amatic+SC:wght@400;700&family=Kaisei+Tokumin:wght@400;700&display=swap"
		rel="stylesheet"
	/>
</svelte:head>
<div class="wrapper">
	<main>
		{#if ctaVisible}
			<header>
				<h1>SiteGeist</h1>
				<div class="subheader">
					<h2>Spectral Social Media.</h2>
					<h2>Don't get ghosted.</h2>
					<button on:click={handleClick}> Pre-register. </button>
				</div>
			</header>
		{:else}
			<section>
				{#if $user}
					<Profile bind:ctaVisible />
				{:else}
					<AuthForm />
				{/if}
			</section>
		{/if}
	</main>
</div>

<style>
	.wrapper {
		height: 100%;
		display: grid;
		align-items: center;
	}

	@media only screen and (min-height: 900px) {
		.wrapper {
			background-image: url('/ghost.svg');
			background-repeat: no-repeat;
			background-position-x: center;
			background-position-y: 3rem;
			background-size: 3rem;
			animation: animatedBackground 2s ease-in-out infinite alternate;
		}
	}

	@keyframes animatedBackground {
		from {
			background-position-y: 3rem;
			background-size: 3.3rem;
		}
		to {
			background-position-y: 4rem;
			background-size: 3rem;
		}
	}

	main {
		display: grid;
		width: 100%;
		max-width: 500px;
		margin: auto;
		row-gap: 3rem;
	}

	.subheader {
		display: grid;
		justify-items: center;
	}
	header h1 {
		text-align: center;
		margin: 0;
		font-size: 7rem;
	}
	header h2 {
		text-align: center;
		margin: 0.5rem 0;
		font-size: 1.5rem;
	}
	.subheader {
		margin: 1rem;
	}
	header button {
		background-color: var(--green);
		border: none;
		padding: 0.5rem 1rem;
		margin-top: 1rem;
		font-weight: 700;
		color: var(--off-black);
		font-size: 1.5rem;
	}
</style>
