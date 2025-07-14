<script lang="ts">
  import downloads from '../../route-downloads.json';

  type Route = { name: string; url: string };
  type Downloads = Record<string, Route[]>;
  const data: Downloads = downloads;

  let selectedLocation: string = Object.keys(data)[0];
</script>

<div class="container mx-auto px-4 py-8">
  <h1 class="text-2xl font-bold mb-6">Download Bus Route PDFs</h1>

  <div class="mb-6 flex flex-wrap gap-2">
    {#each Object.keys(data) as location}
      <button
        class="px-4 py-2 rounded border font-semibold transition-colors duration-150
          {selectedLocation === location
            ? 'bg-blue-600 text-white border-blue-700'
            : 'bg-white text-blue-700 border-blue-300 hover:bg-blue-50'}"
        on:click={() => selectedLocation = location}
      >
        {location}
      </button>
    {/each}
  </div>

  <section>
    <h2 class="text-xl font-semibold mb-2">{selectedLocation}</h2>
    <ul class="ml-4 list-disc">
      {#each data[selectedLocation] as route}
        <li>
          <a
            href={route.url}
            target="_blank"
            rel="noopener"
            class="text-blue-600 hover:underline"
            >{route.name}</a
          >
        </li>
      {/each}
    </ul>
  </section>
</div>