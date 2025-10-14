<script lang="ts">
  import { onMount } from 'svelte';
  import { tick } from 'svelte';
  import type { MainDeparturePoint, SecondaryDeparturePoint} from '../types/BusRoute';

  export let mainDepartPoint : MainDeparturePoint;
  export let secondaryDepartPoint : SecondaryDeparturePoint;

  let mainIndex = 0;
  let secondaryIndex = 0;
  let divRef: HTMLDivElement;

  function convertToSeconds(time: string): number {
    const arr = time.split(':');
    arr[2] = '00';
    return Number(arr[0]) * 3600 + Number(arr[1]) * 60 + Number(arr[2]);
  }

  function highlight(departureTimes: string[]): number {
    const now = new Date();
    const currentTime = now
      .toLocaleTimeString('en-GB', { hour12: false })
      .split(':')
      .slice(0, 2)
      .join(':') + ':00';

    const currentTimeInSeconds = convertToSeconds(currentTime);

    if (departureTimes.length === 0) return 0;

    if (currentTimeInSeconds <= convertToSeconds(departureTimes[0])) return 0;

    if (
      currentTimeInSeconds >=
      convertToSeconds(departureTimes[departureTimes.length - 1])
    )
      return departureTimes.length - 1;

    for (let i = 1; i < departureTimes.length; i++) {
      const prev = convertToSeconds(departureTimes[i - 1]);
      const curr = convertToSeconds(departureTimes[i]);
      if (currentTimeInSeconds > prev && currentTimeInSeconds <= curr) return i;
    }

    return 0;
  }

  function updateIndices() {
    mainIndex = highlight(mainDepartPoint.departureTimes);
    secondaryIndex = highlight(secondaryDepartPoint.departureTimes);
  }

  async function scrollToIndex() {
    await tick();
    divRef?.scrollTo({
      top: (mainIndex - 1) * 24,
      behavior: 'instant'
    });
  }

  onMount(() => {
    updateIndices();
    scrollToIndex();

    const interval = setInterval(() => {
      updateIndices();
    }, 2 * 60 * 1000); // every 2 minutes

    return () => clearInterval(interval);
  });

  $: if (mainDepartPoint) {
    updateIndices();
    scrollToIndex();
  }
</script>

<div>
  <p class="text-xl">Gautrain Bus Schedule:</p>
  <div class="grid grid-cols-2">
    <p>{mainDepartPoint.name}</p>
    <p>{secondaryDepartPoint.name}</p>
    <div
      bind:this={divRef}
      class="col-span-2 grid grid-cols-2 h-24 overflow-y-scroll"
    >
      <div>
        {#each mainDepartPoint.departureTimes as time, index}
          <p class:bg-green-200={mainIndex === index}>{time}</p>
        {/each}
      </div>
      <div>
        {#each secondaryDepartPoint.departureTimes as time, index}
          <p class:bg-green-200={secondaryIndex === index}>{time}</p>
        {/each}
      </div>
    </div>
  </div>
</div>

<style>
 /* Target the scrollbar of the specific div */
.overflow-y-scroll::-webkit-scrollbar {
 width: 10px; /* Make the scrollbar wider */

}

/* Style the scrollbar track (the background) */
.overflow-y-scroll::-webkit-scrollbar-track {
 background: #f1f1f1;
 border-radius: 10px;
}

 /* Style the scrollbar handle (the "thumb" or "wheel") */
.overflow-y-scroll::-webkit-scrollbar-thumb {
background-color: #888; /* Color of the handle */
border-radius: 10px; /* Rounded corners for the handle */

}

/* Add hover effect for the handle */
.overflow-y-scroll::-webkit-scrollbar-thumb:hover {
background-color: #555;
}




</style>
