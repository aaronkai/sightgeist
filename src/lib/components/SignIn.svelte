<script>
	import { supabase } from '$lib/supabaseClient';

	let email;
	let password;
	let message;
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
			message = error.error_description || error.message;
		} finally {
			loading = false;
		}
	}
</script>

<form on:submit|preventDefault={handleSignin}>
	<h1>Sign in</h1>
	<p>Sign in with a username and password</p>
	<input type="email" placeholder="Your email" bind:value={email} />
	<input type="password" placeholder="Your password" bind:value={password} />
	<input type="submit" value={loading ? 'Loading' : 'Sign In'} disabled={loading} />
</form>

{#if message}
	<p>{message}</p>
{/if}
