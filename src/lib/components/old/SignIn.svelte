<script>
	import { fade } from 'svelte/transition';
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

<form id="form" on:submit|preventDefault={handleSignin} transition:fade>
	<header>
		<h1>Sign In</h1>
		<h2>What's up Ghouls?!</h2>
	</header>
	<label for="email"
		>Email:
		<input
			id="email"
			type="email"
			placeholder="Your email"
			autocomplete="email"
			bind:value={email}
		/>
	</label>
	<label for="password"
		>Password:
		<input
			type="password"
			placeholder="Your password"
			autocomplete="current-password"
			bind:value={password}
		/>
	</label>
	<input class="submit" type="submit" value={loading ? 'Loading' : 'Sign In'} disabled={loading} />
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
		margin-bottom: 1rem;
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
		margin-top: 1rem;
		font-weight: 700;
		color: var(--off-black);
	}
	.submit:hover {
		background-color: var(--yellow);
	}

	button {
		color: var(--pink);
		background: none;
		border: none;
		font-weight: 700;
		font-size: 1rem;
		padding: 0;
	}
	button:hover {
		color: var(--yellow);
	}
</style>
