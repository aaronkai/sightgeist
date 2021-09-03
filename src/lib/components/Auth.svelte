<script>
	import SignIn from '$lib/components/SignIn.svelte';
	import SignUp from '$lib/components/SignUp.svelte';

	let newUser = false;
</script>

{#if newUser}
	<SignUp />
	<button on:click={() => (newUser = false)}>Already have an account?</button>
{:else}
	<SignIn />
	<button on:click={() => (newUser = true)}>Do you need to sign up?</button>
{/if}
