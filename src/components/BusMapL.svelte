<script lang="ts">
	import { Map, TileLayer, Marker, Polyline, Icon, Popup } from 'sveaflet';
	import type { Stop } from '../types/BusRoute';

	import busStopIcon2 from '../assets/bus-stop-2.png';
	import busIcon from '../assets/busIcon4.png';
	import busStation from '../assets/gautrainlogo.png';
	import { getBusesOnRoute } from '../services/BusService';
	import { onMount } from 'svelte';
	import type { BusPosition } from '../types/BusPositions';

	export let stops: Stop[];
	export let stationCoordinates: number[];
	export let path: number[][];
	export let station: string;
	

	let busData: BusPosition[] = [];
	let center = [...stationCoordinates];

	const updateBuses = async () => {
    const response = await getBusesOnRoute();
    if (response?.Result?.busPositions) {
      busData = response.Result.busPositions;
  }};

  onMount(() => {
	updateBuses();
	const interval = setInterval(updateBuses, 15000);
	return () => clearInterval(interval);
  });
</script>

<div style="width: 300px;height: 500px;">
	<Map options={{ center, zoom: 14 }}>
		<TileLayer url={'https://tile.openstreetmap.org/{z}/{x}/{y}.png'} />

		<!-- station -->
		<Marker 
			latLng={stationCoordinates}>
			<Icon options={{ iconUrl: busStation, iconSize: [30, 30]}} />
			<Popup options={{content: station + " Gautrain Station"}} />
		</Marker>
		
		<!-- route path -->
		<Polyline 
		latLngs={path.map(([lng, lat]) => [lat, lng])} 
		options={{ color: 'DodgerBlue', weight: 8 }}
		/>

		<!-- bus stops -->
		{#each stops as stop}
			<Marker 
			latLng={stop.latLng}>
			<Icon options={{ iconUrl: busStopIcon2, iconSize: [25, 25]}} />
			<Popup options={{content: stop.code + '<br/>' + stop.street}} />
			</Marker>
		{/each}

		<!-- bus markers -->
		{#each busData as bus}
			<Marker 
			latLng={[bus.latitude, bus.longitude]}>
			<Icon options={{ iconUrl: busIcon, iconSize: [25, 25]}} />
			</Marker>
		{/each}
	</Map>
</div>