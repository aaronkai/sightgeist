<script>
	import Auth from '$lib/components/Auth.svelte';
	import SignOut from '$lib/components/SignOut.svelte';
	import Profile from '$lib/components/Profile.svelte';
	import { user } from '$lib/stores/sessionStore';
	import { supabase } from '$lib/supabaseClient';

	user.set(supabase.auth.user());

	supabase.auth.onAuthStateChange((_, session) => {
		user.set(session.user);
	});
</script>

<h1>SightGeist</h1>
<nav><SignOut /></nav>
<!-- <Auth /> -->

{#if $user}
	<Profile />
{:else}
	<Auth />
{/if}
