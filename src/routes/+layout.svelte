<script lang="ts">
	import { dev } from '$app/environment';
	import '../app.css';
	import Header from '../components/Header.svelte';
	import { injectAnalytics } from '@vercel/analytics/sveltekit'

	let { children } = $props();
	injectAnalytics({ mode: dev ? 'development' : 'production' });
</script>

<svelte:head>
	<title>Gautrain Bus Schedule</title>
	<link rel="apple-touch-icon" sizes="180x180" href="/favicon_io/apple-touch-icon.png">
	<link rel="icon" type="image/svg+xml" href="/bus.svg">
	<link rel="manifest" href="/favicon_io/site.webmanifest">
	<meta name="og:title" content="Gautrain Bus Schedule" />
	<meta name="description" content="Track the Gautrain bus for your area and get the bus schedule for the selected route." />
</svelte:head>
<Header />
{@render children()}
