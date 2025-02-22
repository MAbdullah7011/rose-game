<script>
  
	import { createEventDispatcher, onMount } from 'svelte'
  import { Progress } from '@skeletonlabs/skeleton-svelte';
	import RoseIconGrey from "./RoseIconGrey.svelte";
  import { roseTypes } from "$lib/roseTypes";
  import * as Tooltip from "$lib/components/ui/tooltip/index.js";
  import * as Popover from "$lib/components/ui/popover/index.js";
  

  //psuedo backend logic
  let isGrowing = $state(false)
  let rose = $state({});
  let growthInterval = $state();
  function startGrowingRose() {
      
         if (!isGrowing) {
          rose = {
           id: Date.now(),
           type: roseTypes[Math.floor(Math.random() * roseTypes.length)],
           progress: 0,
           startTime: Date.now(),
           note: ''
         };
          
          
         
         }
         isGrowing=true
         growthInterval = setInterval(updateProgress, 100);
      }
      function stopGrowing() {
  
  if (growthInterval) {
    clearInterval(growthInterval);
    growthInterval = null;
  }
}
const dispatch = createEventDispatcher()

function updateValue() {
    dispatch('updateValue', rose)
    isGrowing=false
}
function updateProgress() {
  if (!isGrowing || !rose) {
    return;
  }

  const elapsed = (Date.now() - rose.startTime) / 1000;
  rose.progress = Math.min(100, (elapsed / rose.type.growthTime) * 100);

  if (rose.progress === 100) {
    stopGrowing();
  }
  }
  
  onMount(() => {
  
  return () => {
    if (growthInterval) {
      clearInterval(growthInterval);
    }
  };
});

</script>


{#if !isGrowing}
  <button onclick={startGrowingRose}>
    <div  class="border-2  p-4 w-28 h-28 flex items-center justify-center rounded-lg border-dashed border-gray-300 hover:border-primary-300 ">
        <RoseIconGrey ></RoseIconGrey>

    </div>
  </button>
  {:else}
  {#if rose.progress==100}
    
    <button onclick={updateValue}>
      <div  class="border-2  p-4 w-28 h-28 flex items-center justify-center rounded-lg border-solid border-primary-300 hover:border-primary-500 hover:bg-gray-50">
        <img class="" width="64" height="64" src={rose.type.path} alt="">
      
      </div>
    </button>
    {:else}
    
      
        <Popover.Root>
          <Popover.Trigger>
            <Tooltip.Provider>
              <Tooltip.Root delayDuration={100}>
                <Tooltip.Trigger>
                  <div  class="border-2  p-4 w-28 h-28 flex items-center justify-center rounded-lg border-solid border-secondary-300 relative">
                    <img class="" width="64" height="64" src={rose.type.path} alt="">
                    <Progress classes='absolute -bottom-5'  value={rose.progress} max={100} meterBg="bg-primary-500 " />
                  </div>
                </Tooltip.Trigger>
                <Tooltip.Content >
                  <p>{Math.round(rose.type.growthTime-((rose.progress*rose.type.growthTime)/100))} seconds left</p>
                </Tooltip.Content>
              </Tooltip.Root>
            </Tooltip.Provider>
          </Popover.Trigger>
          <Popover.Content side='top' class="w-fit">
            <p>{Math.round(rose.type.growthTime-((rose.progress*rose.type.growthTime)/100))} seconds left</p>
          </Popover.Content>
        </Popover.Root>
        
        
      
    
    
  {/if}
{/if}

  