<script>
	import Auth from '$lib/components/Auth.svelte';
	import SignOut from '$lib/components/SignOut.svelte';
	import Profile from '$lib/components/Profile.svelte';
	import { user } from '$lib/stores/sessionStore';
	import { supabase } from '$lib/supabaseClient';
	import '$lib/styles/reset.css';
	import '$lib/styles/global.css';

	user.set(supabase.auth.user());

	supabase.auth.onAuthStateChange((_, session) => {
		user.set(session.user);
	});

	console.log({ $user });
</script>

<div class="wrapper">
	<main>
		<header>
			<h1>SightGeist</h1>
			<h2>Pre-register for a spectral social media experience.</h2>
		</header>

		<section>
			{#if $user}
				<SignOut />
			{/if}

			{#if $user}
				<Profile />
			{:else}
				<Auth />
			{/if}
		</section>
	</main>
</div>

<style>
	.wrapper {
		height: 100%;
		display: grid;
		align-items: center;
	}
	main {
		display: grid;
		max-width: 500px;
		margin: auto;
		row-gap: 3rem;
	}

	header h1 {
		margin-bottom: 1rem;
		text-align: center;
	}
	header h2 {
		text-align: center;
	}
</style>
