<script>
	import { supabase } from '$lib/supabaseClient';
	import ErrorMessage from './ErrorMessage.svelte';

	export let newUser;
	let email;
	let password;
	let message;
	let loading = false;

	console.log(`component level newUser: ${newUser}`);

	async function handleSignup() {
		try {
			loading = true;
			const { user, session, error } = await supabase.auth.signUp({
				email,
				password
			});

			if (error) {
				throw error;
			} else {
				message = 'Check your email for a verification link.';
			}
		} catch (error) {
			message = error.error_description || error.message;
		} finally {
			loading = false;
		}
	}
</script>

<header>
	<h1>Sign Up Now</h1>
	<h2>Don't be the last ghoul to the graveyard. Sign up before this chance apparates!</h2>
</header>

{#if message}
	<ErrorMessage error={message} />
{/if}

<form on:submit|preventDefault={handleSignup}>
	<label for="email"
		>Email:
		<input id="email" type="email" placeholder="Your email" bind:value={email} />
	</label>
	<label for="password"
		>Password:
		<input id="password" type="password" placeholder="Choose a password" bind:value={password} />
	</label>
	<input class="submit" type="submit" value={loading ? 'Loading' : 'Sign Up'} disabled={loading} />
</form>
<button on:click={() => (newUser = false)}>Already have an account?</button>

<style>
	header {
		margin-bottom: 2rem;
		text-align: center;
	}
	header h1 {
		font-size: 3rem;
	}
	form {
		margin: 1rem 0;

		display: grid;
		padding: 2rem;
		row-gap: 1rem;
		border: 2px solid var(--current-line);
	}

	form * {
		font-size: 1.2rem;
	}

	label {
		display: grid;
		grid-template-columns: 1fr 3fr;
		align-items: baseline;
	}
	input {
		border: none;
		background-color: var(--off-black);
		border-bottom: 2px solid var(--pink);
		color: var(--off-white);
	}
	.submit {
		background-color: var(--green);
		border: none;
		padding: 0.5rem 1rem;
		font-weight: 700;
		color: var(--off-black);
	}

	button {
		background-color: var(--pink);
		border: none;
		color: var(--off-black);
		padding: 0.5rem 1rem;
	}
</style>
