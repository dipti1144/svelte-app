<script>
    import { createEventDispatcher } from 'svelte';
    import { data2 } from '../Data/data2'; 
    
  
    let title = '';
    let description="";
   

    

  
    const dispatch = createEventDispatcher();
  
    function addCard() {
      const newCard = { id: data2.length + 1, title , description}; 
      
      dispatch('closeModal', { title , description });
      title = ''; 
  
      const newData = JSON.parse(localStorage.getItem('NestedData')) || [];
      newData.push(newCard);
      localStorage.setItem('NestedData', JSON.stringify(newData));
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
      <div class="mb-4">
        <label for="title" class="block text-gray-700">Description:</label>
        <input type="text" id="title" class="p-2 border border-gray-300 w-full" bind:value={description} />
      </div>
      <div class="flex justify-end">
        <button class="px-4 py-2 bg-gray-800 text-white" on:click={addCard}>Add</button>
        <button class="ml-2 px-4 py-2 bg-gray-300" on:click={() => dispatch('closeModal')}>Cancel</button>
      </div>
    </div>
  </div>
  