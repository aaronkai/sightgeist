<script>
	import { supabase } from '$lib/supabaseClient';

	let email;
	let password;
	let message;
	let loading = false;

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

<form on:submit|preventDefault={handleSignup}>
	<h1>Sign Up</h1>
	<p>Sign Up with a username and password</p>
	<input type="email" placeholder="Your email" bind:value={email} />
	<input type="password" placeholder="Choose and password" bind:value={password} />
	<input type="submit" value={loading ? 'Loading' : 'Sign Up'} disabled={loading} />
</form>

{#if message}
	<p>{message}</p>
{/if}
