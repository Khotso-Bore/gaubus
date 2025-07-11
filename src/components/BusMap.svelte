<script lang="ts">
  import { onMount } from 'svelte';
  import L from 'leaflet';
  import 'leaflet/dist/leaflet.css';
  import { getBusesOnRoute } from '../services/BusService';

  import busStopIcon2 from '../assets/bus-stop-2.png';
  import busIcon from '../assets/busIcon4.png';
  import busStation from '../assets/gautrainlogo.png';
	import type { BusPosition } from '../types/BusPositions';
	import type { Stop } from '../types/BusRoute';

  export let stops: Stop[];
  export let stationCoordinates: number[];
  export let path: number[][];
  export let station: string;

  let map: L.Map;
  let busMarkers: L.Marker[] = [];
  let busData: BusPosition[] = [];
  let center = [...stationCoordinates];

  const busStopIcon = new L.Icon({ iconUrl: busStopIcon2, iconSize: [25, 25] });
  const busMarkerIcon = new L.Icon({ iconUrl: busIcon, iconSize: [25, 25] });
  const stationIcon = new L.Icon({ iconUrl: busStation, iconSize: [30, 30] });

  const initMap = () => {
    console.log('Initializing map with stops:', stationCoordinates);
    console.log('Initializing map with center:', center);
    console.log('Initializing map with station:', station);
    map = L.map('map').setView(center, 14);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);

    // Route path
    L.polyline(path.map(([lng, lat]) => [lat, lng]), {
      color: 'DodgerBlue',
      weight: 8
    }).addTo(map);

    // Stops
    stops.forEach((stop) => {
      L.marker([stop.latLng[0], stop.latLng[1]], { icon: busStopIcon })
        .addTo(map)
        .bindPopup(`${stop.code}<br/>${stop.street}`);
    });

    // Station marker
    L.marker(center, { icon: stationIcon })
      .addTo(map)
      .bindPopup(`${station} Gautrain Station`);
  };

  const updateBuses = async () => {
    const response = await getBusesOnRoute();
    if (response?.Result?.busPositions) {
      busData = response.Result.busPositions;

      // Clear old markers
      busMarkers.forEach((marker) => map.removeLayer(marker));
      busMarkers = [];

      busData.forEach((bus) => {
        const marker = L.marker([bus.latitude, bus.longitude], {
          icon: busMarkerIcon
        }).addTo(map);
        busMarkers.push(marker);
      });
    }
  };

  onMount(() => {
    initMap();
    updateBuses();
    const interval = setInterval(updateBuses, 15000);
    return () => clearInterval(interval);
  });
</script>

<style>
  #map {
    height: 500px;
    width: 300px;
  }
</style>

<div id="map"></div>
