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

<form use:getProfile on:submit|preventDefault={updateProfile}>
	<header>
		<h1>Profile</h1>
		<h2>Get started creating your profile.</h2>
	</header>
	<label for="email">
		Email:
		<input id="email" type="text" value={$user.email} disabled />
	</label>
	<label for="username">
		Name:
		<input id="username" type="text" bind:value={username} />
	</label>
	<label for="website">
		Website:
		<input id="website" type="text" bind:value={website} />
	</label>
	<input
		class="submit"
		type="submit"
		value={loading ? 'Loading ...' : 'Update'}
		disabled={loading}
	/>
	<SignOut />
</form>

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
		margin: 1rem;
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
		width: 100%;
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
