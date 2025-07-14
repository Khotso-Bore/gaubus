<script lang="ts">
	import { dev } from '$app/environment';
	import '../app.css';
	import Header from '../components/Header.svelte';
	import { injectAnalytics } from '@vercel/analytics/sveltekit'

	let { children } = $props();
	injectAnalytics({ mode: dev ? 'development' : 'production' });
</script>

<Header />
{@render children()}
