<script lang="ts">
	import { dev } from '$app/environment';
	import '../app.css';
	import Header from '../components/Header.svelte';
	import { injectAnalytics } from '@vercel/analytics/sveltekit';

	let { children } = $props();
	injectAnalytics({ mode: dev ? 'development' : 'production' });
</script>

<svelte:head>
	<title>Gautrain Bus Routes & Schedule | Live Tracking</title>

	<!-- Favicon Icons -->

	<link rel="icon" href="/favicon.ico" sizes="any" />
	<link rel="icon" type="image/svg+xml" href="/bus.svg" />
	<link rel="icon" type="image/png" sizes="48x48" href="/bus-48.png" />
	<link rel="icon" type="image/png" sizes="72x72" href="/bus-72.png" />
	<link rel="icon" type="image/png" sizes="96x96" href="/bus-96.png" />
	<link rel="icon" type="image/png" sizes="144x144" href="/bus-144.png" />
	<link rel="icon" type="image/png" sizes="152x152" href="/bus-152.png" />
	<link rel="icon" type="image/png" sizes="180x180" href="/bus-180.png" />
	<link rel="icon" type="image/png" sizes="192x192" href="/bus-192.png" />
	<link rel="icon" type="image/png" sizes="384x384" href="/bus-384.png" />
	<link rel="icon" type="image/png" sizes="512x512" href="/bus-512.png" />

	<!-- Apple Touch Icon (required for iOS home screen install) -->
	<link rel="apple-touch-icon" sizes="180x180" href="/bus-180.png" />

	<!-- Manifest -->
	<link rel="manifest" href="site.webmanifest" />

	<!-- Theme Colors (affects mobile browser UI + PWA appearance) -->
	<!-- <meta name="theme-color" content="#ffffff" />
	<meta name="apple-mobile-web-app-status-bar-style" content="white-translucent" />
	<meta name="apple-mobile-web-app-capable" content="yes" /> -->
	<meta name="google-adsense-account" content="ca-pub-6775987530927779" />

	<!-- PWA specific meta -->
	<!-- <meta name="mobile-web-app-capable" content="yes" />
	<meta name="application-name" content="Gautrain Bus Schedule" />
	<meta name="apple-mobile-web-app-title" content="Gautrain Bus" /> -->

	<!-- Social Sharing -->
	<meta property="og:title" content="Gautrain Bus Routes & Schedule | Live Tracking" />
	<meta
		property="og:description"
		content="Find Gautrain bus routes, schedules, and live tracking information. Plan your journey with the latest Gautrain bus timetables and stop locations."
	/>
	<meta
		name="description"
		content="Find Gautrain bus routes, schedules, and live tracking information. Plan your journey with the latest Gautrain bus timetables and stop locations."
	/>
</svelte:head>

<Header />
{@render children()}
