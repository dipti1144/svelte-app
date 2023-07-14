<script>
  import Sidebar from "../Components/Sidebar.svelte";
  import Modal from "../Components/Modal.svelte";
  import NestedModal from "../Components/NestedModal.svelte";
  import { onMount, createEventDispatcher } from "svelte";
  import { data2 } from "../Data/data2";
  import { scale } from "svelte/transition";

  let showSidebar = true;
  let showModal = false;
  let showNestedModal=false
  let selectedCard = null;
  let nestedData = [];
  let searchText = "";

  function toggleSidebar() {
    showSidebar = !showSidebar;
  }

  function toggleModal() {
    showModal = !showModal;
  }

  function toggleNestedModal(){
    showNestedModal=!showNestedModal
  }

  function handleSelectCard(event) {
    selectedCard = event.detail;
    console.log(selectedCard)
    nestedData = data2[selectedCard.id] || [];
    console.log(nestedData)
  }

  function handleSearch(event) {
    searchText = event.target.value;
  }

  const dispatch = createEventDispatcher();


  function opennestedModal(){
    dispatch("openModal")
    showNestedModal=true
  }

  onMount(() => {
    dispatch("selectCard", selectedCard);
  });
</script>

<div class="flex">
  <div>
    <div
      class={`flex  bg-blue-900  h-screen ${
        showSidebar ? "w-74" : "w-20"
      }  p-7 pt-8   relative `}
    >
      <div class={`${!showSidebar && "scale-0"}`}>
        <Sidebar on:openModal={toggleModal} on:selectCard={handleSelectCard} />
      </div>
      <div class="cursor-pointer">
        <img
          class={`p-2 bg-white w-7 rounded-full ml-3 border-2 absolute -right-3 top-10 ${
            !showSidebar && "rotate-180"
          } `}
          src="https://cdn-icons-png.flaticon.com/512/271/271220.png"
          on:click={toggleSidebar}
        />
      </div>
    </div>
  </div>

  <div class="p-8">
    <div>
      <button on:click={opennestedModal}  class="p-2 pl-4 pr-4 bg-blue-800 text-white "  >Add Item</button>
    </div>
    <h1 class="text-2xl font-semibold mb-4">Main Content</h1>
    {#if selectedCard}
      <h2 class="text-xl font-semibold mb-2">{selectedCard.title}</h2>
      <input
        type="text"
        placeholder="Search Nested Data"
        class="p-2 border border-gray-300 w-64 mb-4"  
        on:input={handleSearch}
      />
      {#if nestedData.length === 0}
        <p>No nested data available.</p>
      {:else}
        {#each nestedData as item (item.id)}
          {#if item.title.toLowerCase().includes(searchText.toLowerCase())}
            <div class="p-4 border-b border-gray-300">
              <p class="text-lg font-medium">{item.title}</p>
              <p class="text-gray-500">{item.description}</p>
            </div>
          {/if}
        {/each}
      {/if}
    {/if}
  </div>
</div>

{#if showModal}
  <Modal on:closeModal={toggleModal} />
{/if}

{#if showNestedModal}
  <NestedModal on:closeModal={toggleNestedModal}   />
{/if}


