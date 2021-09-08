<script>
	import SignIn from '$lib/components/SignIn.svelte';
	import SignUp from '$lib/components/SignUp.svelte';

	let newUser = true;
	console.log({ newUser });
</script>

{#if newUser}
	<SignUp bind:newUser />
{:else}
	<SignIn bind:newUser />
{/if}
