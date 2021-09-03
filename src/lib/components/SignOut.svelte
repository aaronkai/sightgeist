<script>
	import { supabase } from '$lib/supabaseClient';

	let message = '';
	let loading = false;
	const user = supabase.auth.user();

	async function signOut() {
		try {
			loading = true;
			let { error } = await supabase.auth.signOut();
			if (error) {
				throw error;
			} else {
				message = 'You have signed out';
			}
		} catch (error) {
			message = `You have signed out`;
			console.error(error);
		} finally {
			loading = false;
		}
	}
</script>

<button on:click={signOut} disabled={loading}> Sign Out </button>

{#if message}
	<p>{message}</p>
{/if}
