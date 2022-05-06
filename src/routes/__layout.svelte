<script>
	import '../app.css';
	import Auth from '../components/Auth.svelte';
	import { supabase } from '../supabase';
	import { user } from '../stores/authStore';
	import { loadTodos } from '../stores/todoStores';
	import Navbar from '../components/Navbar.svelte';

	console.log(supabase.auth.user());
	user.set(supabase.auth.user());

	supabase.auth.onAuthStateChange((_, session) => {
		user.set(session?.user);
		if (session?.user) {
			loadTodos();
		}
	});
</script>

<svelte:head>
	<title>Todo App</title>
</svelte:head>

<div class="container mx-auto my-6 max-w-lg">
	{#if $user}
		<Navbar />
		<slot />
	{:else}
		<Auth />
	{/if}
</div>
