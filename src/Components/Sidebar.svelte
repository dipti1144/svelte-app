<script>
  import { onMount, createEventDispatcher } from "svelte";
  import { data } from "../Data/data";

  let searchQuery = "";
  let filteredData = data;

  const dispatch = createEventDispatcher();

  function search() {
    filteredData = data.filter((item) =>
      item.title.toLowerCase().includes(searchQuery.toLowerCase())
    );
  }

  function openModal() {
    dispatch("openModal");
  }

  function selectCard(card) {
    dispatch("selectCard", card);
  }

  onMount(() => {
    search();

    const localStorageData = localStorage.getItem("newData");
    if (localStorageData) {
      const newData = JSON.parse(localStorageData);
      data.push(...newData);
      filteredData = data;
    }
  });
</script>

<div>
  <input
    type="text"
    placeholder="Search"
    class="p-2 border border-gray-300 w-full"
    bind:value={searchQuery}
    on:input={search}
  />
  {#if filteredData.length === 0}
    <p class="p-4 text-blue-100">No results found.</p>
  {:else}
    {#each filteredData as item (item.id)}
      <div
        class="p-4 border-b-2 border-blue-600"
        on:click={() => selectCard(item)}
      >
        <p class="cursor-pointer text-blue-100">{item.title}</p>
      </div>
    {/each}
  {/if}

  <button
    class="fixed bottom-2 left-2 p-2 bg-gray-800 text-white"
    on:click={openModal}
  >
    Add New Card
  </button>
</div>
