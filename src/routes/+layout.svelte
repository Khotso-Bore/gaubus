<script lang="ts">
	import { dev } from '$app/environment';
	import '../app.css';
	import Header from '../components/Header.svelte';
	import { injectAnalytics } from '@vercel/analytics/sveltekit'

	let { children } = $props();
	injectAnalytics({ mode: dev ? 'development' : 'production' });
</script>

<svelte:head>
	<title>Gaubus Schedule</title>
	<link rel="apple-touch-icon" sizes="180x180" href="/favicon_io/apple-touch-icon.png">
	<link rel="icon" type="image/png" sizes="48x48" href="/favicon_io/favicon.ico">
	<link rel="manifest" href="/favicon_io/site.webmanifest">
	<meta name="og:title" content="Gaubus Schedule" />
	<meta name="description" content="This application tracks the gaubus (gautrain bus) and shows the schedule for the selected route." />
</svelte:head>
<Header />
{@render children()}
