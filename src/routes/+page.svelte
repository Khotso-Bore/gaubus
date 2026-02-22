<script lang="ts">
	import { onMount } from 'svelte';
	import MainSchedule from '../components/MainSchedule.svelte';
	import BusMap from '../components/BusMap.svelte';
	import { getBusRoute } from '../services/BusService';
	import type { BusRoute } from '../types/BusRoute';
	import BusMapL from '../components/BusMapL.svelte';
	import busStopIcon2 from '../assets/bus-stop-2.png';
	import busIcon from '../assets/busIcon4.png';
	import busStation from '../assets/gautrainlogo.png';

	$: city = 'Midrand';
	let area: string = 'Noordwyk';
	let busRoute: BusRoute | undefined;

	$: areas = stations.find((x) => x.city === city)?.areas ?? [];

	const stations = [
		{
			city: 'Midrand',
			latLng: [-25.995422983347027, 28.1359387563011],
			areas: ['Noordwyk', 'Randjespark', 'Sunninghill', 'mall of africa']
		},
		{
			city: 'Centurion',
			latLng: [-25.851703936077403, 28.189615067815687],
			areas: ['Rooihuiskraal', 'Techno Park', 'Southdowns', 'Wierdapark', 'Highveld', 'Midstream']
		},
		{
			city: 'Hatfield',
			latLng: [-25.746644695069936, 28.23931115432058],
			areas: ['Arcadia', 'Brooklyn', 'Lynnwood', 'Menlyn', 'Queenswood']
		},
		{
			city: 'Park',
			latLng: [-26.194812299621333, 28.042511625499476],
			areas: ['CBD', 'Parktown']
		},
		{
			city: 'Pretoria',
			latLng: [-25.759117962291562, 28.19019196781267],
			areas: ['CBD', 'Zoo']
		},
		{
			city: 'Rhodesfield',
			latLng: [-26.12798788479639, 28.22503272549707],
			areas: ['Emperors Palace', 'Kempton Park CBD', 'Kempton Park Express']
		},
		{
			city: 'Rosebank',
			latLng: [-26.147179529926458, 28.045109412006106],
			areas: ['Hyde Park', 'Illovo', 'Killaney', 'Melose Arch']
		},
		{
			city: 'Sandton',
			latLng: [-26.10769179415422, 28.05726319666034],
			areas: ['Fourways', 'Gallo Manor', 'Randburg', 'Rivonia']
		},
		{
			city: 'Marlboro',
			latLng: [-26.083303651687363, 28.11096623898717],
			areas: ['Kelvin', 'Buccleuch', 'Lakeside', 'Linbro', 'Greenstone', 'Woodlands Office Park']
		}
	];

	const cities = stations.map((x) => x.city);

	// Fetch data on mount and when area/city changes
	async function fetchRoute() {
		const response = await getBusRoute(city, area);
		if (response) {
			busRoute = response;
			console.log('cities:', cities);
			console.log('areas:', areas);
			console.log('Bus Route Data:', city, area);
			console.log('Bus Route Here:', stations.find((x) => x.city === city)?.latLng as number[]);
		}
	}

	onMount(async () => {
		await fetchRoute();
	});

	//$: area, fetchRoute();
</script>

{#if busRoute}
	<div class="flex flex-col items-center gap-2">
		<select
			bind:value={city}
			class="custom-select-arrow mt-6 w-3/4 rounded-lg border-2 border-blue-400 bg-white px-4 py-2 text-gray-800 shadow transition focus:ring-2 focus:ring-blue-300 focus:outline-none"
		>
			{#each cities as city}
				<option value={city}>{city}</option>
			{/each}
		</select>

		<select
			bind:value={area}
			class="custom-select-arrow mt-2 w-3/4 rounded-lg border-2 border-blue-400 bg-white px-4 py-2 text-gray-800 shadow transition focus:ring-2 focus:ring-blue-300 focus:outline-none"
			onchange={async () => {
				await fetchRoute();
			}}
		>
			{#each areas as area}
				<option value={area}>{area}</option>
			{/each}
		</select>

		{#key busRoute}
			<MainSchedule
				mainDepartPoint={busRoute.mainDeparturePoint}
				secondaryDepartPoint={busRoute.secondaryDeparturePoint}
			/>

			{#key city}
				<!-- <BusMap
        stops={busRoute.stops}
        stationCoordinates={stations.find(x => x.city === city)?.latLng ?? [0, 0]}
        path={busRoute.path}
        station={city}
      /> -->
				<BusMapL
					stops={busRoute.stops}
					stationCoordinates={stations.find((x) => x.city === city)?.latLng ?? [0, 0]}
					path={busRoute.path}
					station={city}
				/>
			{/key}
		{/key}
	</div>
{:else}
	<div>Loading Data...</div>
{/if}

<div class="mt-8 mb-4 rounded-lg px-6 py-4 text-center text-lg font-medium shadow">
	<h1 class="text-xl font-bold text-blue-600">Gautrain Bus Routes & Schedule | Live Tracking</h1>
	<div>
		Tracks the Gautrain bus (Gaubus) and shows the schedule for the selected route. Choose a city
		and area to view the current bus route, bus stops, the busses enroute and timetable information.
		Bus locations are updated every 15 seconds.
	</div>
	<div class="mt-4 flex flex-col items-center gap-2">
		<h2 class="mb-2 text-lg font-semibold text-blue-700">Map Legend</h2>
		<div class="flex items-center gap-2">
			<img src={busIcon} alt="Bus Icon" class="h-6 w-6" />
			<span>Represents the bus on the map.</span>
		</div>
		<div class="flex items-center gap-2">
			<img src={busStopIcon2} alt="Bus Stop Icon" class="h-6 w-6" />
			<span>Represents bus stops on the map.</span>
		</div>
		<div class="flex items-center gap-2">
			<img src={busStation} alt="Gautrain Station Icon" class="h-6 w-6" />
			<span>Represents the Gautrain station on the map.</span>
		</div>
	</div>
</div>

<style>
	/* Custom style for select arrow with thicker border */
	.custom-select-arrow {
		appearance: none;
		-webkit-appearance: none;
		-moz-appearance: none;
		background-image:
			linear-gradient(45deg, transparent 49%, #2563eb 51%),
			linear-gradient(135deg, #2563eb 51%, transparent 49%);
		background-position:
			calc(100% - 1.5em) calc(1em + 2px),
			calc(100% - 1em) calc(1em + 2px);
		background-size:
			0.5em 0.5em,
			0.5em 0.5em;
		background-repeat: no-repeat;
		padding-right: 2.5em;
		/* Add border to the arrow area */
		position: relative;
	}
</style>
