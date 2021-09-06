<script>
	import { supabase } from '$lib/supabaseClient';

	export let newUser;
	let email;
	let password;
	let message;
	let errorMessage;
	let loading = false;

	async function handleSignin() {
		try {
			loading = true;
			const { user, session, error } = await supabase.auth.signIn({
				email,
				password
			});
			if (error) {
				throw error;
			} else {
				message = 'You have logged in!';
			}
		} catch (error) {
			errorMessage = error.error_description || error.message;
		} finally {
			loading = false;
		}
	}
</script>

<form on:submit|preventDefault={handleSignin}>
	<h1>Sign in</h1>
	<p>Sign in with a username and password</p>
	<input type="email" placeholder="Your email" autocomplete="email" bind:value={email} />
	<input
		type="password"
		placeholder="Your password"
		autocomplete="current-password"
		bind:value={password}
	/>
	<input type="submit" value={loading ? 'Loading' : 'Sign In'} disabled={loading} />
</form>
<button on:click={() => (newUser = true)}>Do you need to sign up?</button>

{#if message}
	<p>Success: {message}</p>
{/if}

{#if errorMessage}
	<p>Error: {errorMessage}</p>
{/if}

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
		padding: 1rem;
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
		padding: 1rem;
	}

	button {
		background-color: var(--pink);
		border: none;
		color: var(--off-black);
	}
</style>
