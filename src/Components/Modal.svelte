<script>
  import { createEventDispatcher } from 'svelte';
  import { data } from '../Data/data'; 

  let title = '';

  const dispatch = createEventDispatcher();

  function addCard() {
    const newCard = { id: data.length + 1, title }; 
    data.push(newCard); 
    dispatch('closeModal', { title });
    title = ''; 

    const newData = JSON.parse(localStorage.getItem('newData')) || [];
    newData.push(newCard);
    localStorage.setItem('newData', JSON.stringify(newData));
    window.location.reload()
  }
</script>

<div class="fixed top-0 left-0 w-full h-full bg-gray-900 bg-opacity-75 flex justify-center items-center">
  <div class="bg-white p-8 rounded shadow-lg">
    <h2 class="text-lg font-semibold mb-4">Add New Card</h2>
    <div class="mb-4">
      <label for="title" class="block text-gray-700">Title:</label>
      <input type="text" id="title" class="p-2 border border-gray-300 w-full" bind:value={title} />
    </div>
    <div class="flex justify-end">
      <button class="px-4 py-2 bg-gray-800 text-white" on:click={addCard}>Add</button>
      <button class="ml-2 px-4 py-2 bg-gray-300" on:click={() => dispatch('closeModal')}>Cancel</button>
    </div>
  </div>
</div>
