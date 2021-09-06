<script>
	import { supabase } from '$lib/supabaseClient';
	import { user } from '$lib/stores/sessionStore';
	import SignOut from './SignOut.svelte';

	let loading = true;
	let username = null;
	let website = null;
	let avatar_url = null;

	async function getProfile() {
		try {
			loading = true;
			const user = supabase.auth.user();
			console.log(user.id);
			let { data, error, status } = await supabase
				.from('profiles')
				.select(`username, website, avatar_url`)
				.eq('id', user.id)
				.single();

			if (error && status !== 406) throw error;
			console.log({ data, error, status });
			if (data) {
				username = data.username;
				website = data.website;
				avatar_url = data.avatar_url;
			}
		} catch (error) {
			alert(error.message);
		} finally {
			loading = false;
		}
	}

	async function updateProfile() {
		try {
			loading = true;
			const user = supabase.auth.user();

			const updates = {
				id: user.id,
				username,
				website,
				avatar_url,
				updated_at: new Date()
			};

			let { error } = await supabase.from('profiles').upsert(updates, {
				returning: 'minimal' // Don't return the value after inserting
			});

			if (error) throw error;
		} catch (error) {
			alert(error.message);
		} finally {
			loading = false;
		}
	}
</script>

<h1>Profile</h1>
<form use:getProfile on:submit|preventDefault={updateProfile}>
	<label for="email">Email</label>
	<input id="email" type="text" value={$user.email} disabled />
	<label for="username">Name</label>
	<input id="username" type="text" bind:value={username} />
	<label for="website">Website</label>
	<input id="website" type="text" bind:value={website} />
	<input type="submit" value={loading ? 'Loading ...' : 'Update'} disabled={loading} />
</form>

<SignOut />
