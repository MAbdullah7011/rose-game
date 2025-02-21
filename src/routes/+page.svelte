<script >
  import { fade, scale } from 'svelte/transition';
  import { onMount } from 'svelte';
  import { Tabs } from '@skeletonlabs/skeleton-svelte';
  import * as Popover from "$lib/components/ui/popover/index.js";
  import { Textarea } from "$lib/components/ui/textarea/index.js";
	import GrowPlaceholder from '$lib/components/GrowingSlot.svelte';
	import Garden from '$lib/components/Garden.svelte';
	import Achievements from '$lib/components/Achievements.svelte';
  
  

  
  
  let group = $state('grow');
  // State management
  let totalRoses = 0;
  let textShake = $state(false);
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
  
  function triggerShakeAnimation() {
    textShake = true;
    setTimeout(() => {
      textShake = false;
    }, 1000);
    console.log('reached') // Reset after animation duration
  }
  // Send a rose to your partner
  
   function sendRose(rose) {
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
  
  
</script>
  

<div class="max-w-sm md:max-w-2xl 2xl:max-w-4xl mx-auto pb-5">
  <!-- Header -->
  <div class="text-center space-y-12 my-8">
    <h1 class="h1 text-primary-300">Our Rose Garden</h1>
    
    <div class='{textShake?"text-error-700 shake":""}'>
      <p>Daily Roses Sent: {dailyRosesSent}/{maxDailyRoses}</p>
    </div>
  </div>
  <Tabs bind:value={group} listClasses="justify-center md:justify-normal" ><!-- add listGap=0 to reduce gap between tabs -->
    {#snippet list()}
      <Tabs.Control value="grow">Grow</Tabs.Control>
      <Tabs.Control value="garden">Garden</Tabs.Control>
      <Tabs.Control value="achievements">Achievements</Tabs.Control>
      <!-- <Tabs.Control value="settings">Settings</Tabs.Control> -->
    {/snippet}
    {#snippet content()}
      <Tabs.Panel value="grow" >
        <div class="border-surface-100 border-b-0 border rounded-lg p-2 pb-12 md:p-6 md:pb-12 shadow-sm shadow-surface-100 mb-8 ">
          
            <div class="flex mb-4 gap-4 md:gap-7 items-center">
              <h5 class="h5  text-primary-300 ">Grow new rose</h5>
              <p class="text-surface-200-800 text-sm font-semibold ">tap on an empty slot to start growing</p>
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
        <div class="border-surface-100 border-b-0  border rounded-lg p-2 pb-12 md:p-6 md:pb-12 shadow-sm shadow-surface-100  min-h-56">
          <div class="flex gap-5 md:gap-7 mb-4 items-center">
            <h5 class="h5 text-primary-300 ">Your Inventory</h5>
            {#if roseCollection.length!==0}
              <p class="text-surface-200-800 text-sm font-semibold p-1 antialiased ">select a rose to send</p>

            {/if}
          </div>
          {#if roseCollection.length==0}
              <p class="text font-bold text-surface-200 text-center mt-12">so empty...</p>
            {/if}
          <div class="flex flex-wrap gap-3 md:gap-4">
            {#each roseCollection as rose (rose.id)}
            <Popover.Root>
              <Popover.Trigger onclick={()=>{(dailyRosesSent>4)&&triggerShakeAnimation()}}>
                
                  <div  class="border-2    p-4 w-28 h-28 flex items-center justify-center rounded-lg border-solid  border-primary-300 hover:border-primary-500 hover:bg-gray-50 " transition:scale>
                    <img class="" width="64" height="64" src={rose.type.path} alt="">
                  
                  </div>
                
              </Popover.Trigger>
              <Popover.Content trapFocus={false} side="top">
                <div class="space-y-1">
                  <p>{rose.type.name}</p>
                  <Textarea disabled={dailyRosesSent>4} bind:value={rose.note} placeholder="Add a note (optional)" maxlength={80} />
                  <div class="flex justify-end"><button disabled={dailyRosesSent>4} onclick={()=>{sendRose(rose)}} type="button" class="btn btn-md preset-filled-primary-500 ">Send</button></div>
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
      <Tabs.Panel value="achievements">
        <Achievements/>
      </Tabs.Panel>
      <!-- <Tabs.Panel value="settings">

      </Tabs.Panel> -->
    {/snippet}
  </Tabs>
  <!-- Growing Area -->
  
</div>


<style>
  

  .shake {
    animation: shake 0.82s cubic-bezier(.36,.07,.19,.97) both;
  }

  @keyframes shake {
    10%, 90% {
      transform: translate3d(-1px, 0, 0);
    }
    
    20%, 80% {
      transform: translate3d(2px, 0, 0);
    }

    30%, 50%, 70% {
      transform: translate3d(-4px, 0, 0);
    }

    40%, 60% {
      transform: translate3d(4px, 0, 0);
    }
  }
</style>