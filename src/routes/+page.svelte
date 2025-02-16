<script>
  import { fade, scale } from 'svelte/transition';
  import { onMount } from 'svelte';
  import { Tabs } from '@skeletonlabs/skeleton-svelte';
  import * as Popover from "$lib/components/ui/popover/index.js";
  import { Textarea } from "$lib/components/ui/textarea/index.js";

	import GrowPlaceholder from '$lib/components/GrowingSlot.svelte';
	import Garden from '$lib/components/Garden.svelte';
  
  let group = $state('grow');
  // State management
  let totalRoses = 0;
  let gardenCollection = $state([])
  let roseCollection = $state([]);
  let dailyRosesSent = $state(0);
  let maxDailyRoses = 5;
  let maxGrowingRoses = 3;
  // Rose types with their rarity and growth time
  
  
  // Start growing a new rose
  function handleUpdate(event) {
    roseCollection = [...roseCollection, event.detail]
  }
  
  // Send a rose to your partner
  
  export function sendRose(rose) {
    if (dailyRosesSent < maxDailyRoses) {
      // Remove from growing roses
      
      dailyRosesSent++;
      
      // TODO: Integration with your backend
      totalRoses++;
      
      // Add to collection
      gardenCollection = [...gardenCollection, rose];
      roseCollection = roseCollection.filter(obj => obj.id !== rose.id);
    }
  }
  
  // Update rose growth progress
  
</script>
  
  
<div class="max-w-sm md:max-w-2xl 2xl:max-w-4xl mx-auto">
  <!-- Header -->
  <div class="text-center space-y-12 my-8">
    <h1 class="h1 text-primary-300">Our Rose Garden</h1>
    
    <p>Daily Roses Sent: {dailyRosesSent}/{maxDailyRoses}</p>
  </div>
  <Tabs bind:value={group} >
    {#snippet list()}
      <Tabs.Control value="grow">Grow</Tabs.Control>
      <Tabs.Control value="garden">Garden</Tabs.Control>
      <Tabs.Control value="achievements">Achievements</Tabs.Control>
    {/snippet}
    {#snippet content()}
      <Tabs.Panel value="grow">
        <div class="bg-white rounded-lg p-2 pb-12 md:p-6 md:pb-12 shadow-lg mb-8">
          
            <div class="flex gap-7 ">
              <h5 class="h5  text-primary-300 mb-4">Grow new rose</h5>
              <p class="text-surface-200 text-sm font-semibold p-1 antialiased ">tap on an empty slot to start growing</p>
            </div>
          
          
            <!-- {#each growingRoses as rose (rose.id)}
              <div 
                class="border rounded-lg p-4 bg-gradient-to-b from-green-50 to-rose-50"
                transition:fade
              >
                <div class="flex justify-between items-center mb-2">
                  <span class="font-medium text-rose-700">{rose.type.name}</span>
                  <span class="text-sm text-rose-500">{Math.floor(rose.progress)}%</span>
                </div>
                <div class="w-full bg-gray-200 rounded-full h-2 mb-4">
                  <div
                    class="bg-rose-500 rounded-full h-2 transition-all duration-1000"
                    style="width: {rose.progress}%"
                  ></div>
                </div>
                {#if rose.progress >= 100}
                  <button
                    onclick={() => sendRose(rose)}
                    class="w-full bg-rose-500 hover:bg-rose-600 text-white rounded-lg px-4 py-2 transition"
                    disabled={dailyRosesSent >= maxDailyRoses}
                  >
                    Send to Partner
                  </button>
                {/if}
              </div>
            {/each} -->
          
          
          <div class="flex justify-between md:justify-normal gap-2 md:gap-8">
            {#each Array(maxGrowingRoses) as _, index (index)}
            
            
            
                <GrowPlaceholder on:updateValue={handleUpdate}></GrowPlaceholder>
            
            
            
            
              {/each}
          </div>
            <!-- <div class="mt-4">
              
              <div class="flex flex-wrap gap-2">
                {#each roseTypes as type}
                  <button
                    onclick={() => startGrowingRose(type)}
                    class="px-4 py-2 rounded-lg bg-{type.color} text-rose-600 hover:opacity-90 transition"
                  >
                    {type.name}
                  </button>
                {/each}
              </div>
            </div> -->
          
        </div>
        
        <!-- Collection -->
        <div class="bg-white rounded-lg p-6 shadow-lg min-h-56">
          <div class="flex gap-7">
            <h5 class="h5 text-primary-300 mb-4">Your Inventory</h5>
            {#if roseCollection.length!==0}
              <p class="text-surface-200 text-sm font-semibold p-1 antialiased ">select a rose to send</p>

            {/if}
          </div>
          {#if roseCollection.length==0}
              <p class="text font-bold text-surface-400 text-center mt-12">so empty...</p>
            {/if}
          <div class="flex flex-wrap gap-4">
            {#each roseCollection as rose (rose.id)}
            <Popover.Root>
              <Popover.Trigger>
                
                  <div  class="border-2    p-4 w-28 h-28 flex items-center justify-center rounded-lg border-solid  border-primary-600  " transition:scale>
                    <img class="" width="64" height="64" src={rose.type.path} alt="">
                  
                  </div>
                
              </Popover.Trigger>
              <Popover.Content trapFocus={false} >
                <div class="space-y-1">
                  <Textarea bind:value={rose.note} placeholder="Add a note" />
                  <div class="flex justify-end"><button onclick={()=>{sendRose(rose)}} type="button" class="btn btn-md preset-filled-primary-500 ">Send</button></div>
                </div>
              </Popover.Content>
            </Popover.Root>
            
            {/each}
          </div>
        </div>
      
      </Tabs.Panel>
      <Tabs.Panel value="garden">
        
          <!-- Outer container with green background and brown border -->
        <Garden gardenCollection={gardenCollection}></Garden>
        
      </Tabs.Panel>
      <Tabs.Panel value="achievements"></Tabs.Panel>
    {/snippet}
  </Tabs>
  <!-- Growing Area -->
  
</div>
  