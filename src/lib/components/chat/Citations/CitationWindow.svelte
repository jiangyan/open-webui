<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  export let citation: any = null;
  export let visible = false;
</script>

<div 
  class="fixed inset-0 bg-black/50 z-[100] flex items-center justify-center"
  class:hidden={!visible}
  on:click|self={() => dispatch('close')}
>
  <div class="bg-white dark:bg-gray-900 rounded-lg w-[600px] max-h-[80vh] overflow-y-auto">
    <div class="flex justify-between items-center p-4 border-b border-gray-200 dark:border-gray-800">
      <h2 class="text-lg font-medium">Citation</h2>
      <button 
        class="text-gray-500 hover:text-gray-700 dark:hover:text-gray-300"
        on:click={() => dispatch('close')}
      >
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
    </div>
    
    <div class="p-4">
      {#if citation && citation.document}
        {#each citation.document as doc, index}
          <div class="mb-6 last:mb-0">
            <!-- Source section -->
            <div class="mb-4">
              <div class="text-sm font-medium text-gray-700 dark:text-gray-300">Source</div>
              <div class="text-sm text-gray-600 dark:text-gray-400">{citation.source.name}</div>
            </div>
            
            <!-- Content section -->
            <div class="mb-4">
              <div class="text-sm font-medium text-gray-700 dark:text-gray-300">Content</div>
              <div class="text-sm text-gray-600 dark:text-gray-400 whitespace-pre-wrap">
                {doc}
              </div>
            </div>

            {#if index < citation.document.length - 1}
              <hr class="my-4 border-gray-200 dark:border-gray-800" />
            {/if}
          </div>
        {/each}
      {/if}
    </div>
  </div>
</div> 