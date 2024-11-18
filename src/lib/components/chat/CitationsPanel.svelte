<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { fly } from 'svelte/transition';
  const dispatch = createEventDispatcher();

  export let citations: any[] = [];
  export let visible = false;

  // Function to truncate content to first 180 chars
  const truncateContent = (text: string, maxLength = 180) => {
    if (text.length > maxLength) {
      return text.substring(0, maxLength) + '...';
    }
    return text;
  };

  // Function to handle citation button click - dispatch event to show citation content
  const handleCitationClick = (citation: any) => {
    dispatch('showCitation', citation);
  };
</script>

{#if citations && citations.length > 0 && visible}
  <div 
    class="citations-panel fixed top-0 right-0 bottom-0 w-[400px] bg-white dark:bg-gray-900 border-l border-gray-200 dark:border-gray-800 overflow-y-auto z-50"
    class:translate-x-full={!visible}
    transition:fly={{ x: 400, duration: 200 }}
  >
    <div class="flex flex-col h-full">
      <div class="flex justify-between dark:text-gray-300 px-5 pt-4 pb-2">
        <div class="text-lg font-medium self-center capitalize">Citations</div>
        <button 
          class="self-center"
          on:click={() => dispatch('close')}
        >
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="w-5 h-5">
            <path d="M6.28 5.22a.75.75 0 00-1.06 1.06L8.94 10l-3.72 3.72a.75.75 0 101.06 1.06L10 11.06l3.72 3.72a.75.75 0 101.06-1.06L11.06 10l3.72-3.72a.75.75 0 00-1.06-1.06L10 8.94 6.28 5.22z" />
          </svg>
        </button>
      </div>
      
      <div class="flex flex-col w-full px-6 pb-5 space-y-6">
        {#if citations && citations.length > 0}
          {#each citations[0].document as doc, docIndex}
            <div class="flex flex-col w-full dark:text-gray-200">
              <!-- Source section -->
              <div class="text-sm font-medium dark:text-gray-300">Source</div>
              <div class="text-sm dark:text-gray-400">
                <div class="flex gap-1 text-xs font-semibold">
                  <button 
                    class="no-toggle flex dark:text-gray-300 py-1 px-1 bg-gray-50 hover:bg-gray-100 dark:bg-gray-850 dark:hover:bg-gray-800 transition rounded-xl max-w-96"
                    on:click={() => handleCitationClick(citations[0])}
                  >
                    <div class="bg-white dark:bg-gray-700 rounded-full size-4">{docIndex + 1}</div>
                    <div class="flex-1 mx-2 line-clamp-1 truncate">{citations[0].metadata[docIndex].name}</div>
                  </button>
                </div>
              </div>

              <!-- Relevance section -->
              <div class="text-sm font-medium dark:text-gray-300 mt-2">Relevance</div>
              <div class="text-sm my-1 dark:text-gray-400 flex items-center gap-2">
                <span class="px-1 rounded font-medium bg-yellow-200 dark:bg-yellow-800 text-yellow-800 dark:text-yellow-200">
                  {((1 - citations[0].distances[docIndex]) * 100).toFixed(2)}%
                </span>
                <span class="text-gray-500 dark:text-gray-500">
                  ({citations[0].distances[docIndex].toFixed(4)})
                </span>
              </div>

              <!-- Content section -->
              <div class="text-sm font-medium dark:text-gray-300 mt-2">Content</div>
              <div class="text-sm dark:text-gray-400 mt-1">
                {truncateContent(doc)}
              </div>

              {#if docIndex < citations[0].document.length - 1}
                <hr class="my-4 border-gray-200 dark:border-gray-800" />
              {/if}
            </div>
          {/each}
        {/if}
      </div>
    </div>
  </div>
{/if}

<style>
  .citations-panel {
    transition: transform 0.2s ease-in-out;
  }
  
  .translate-x-full {
    transform: translateX(100%);
  }
</style> 