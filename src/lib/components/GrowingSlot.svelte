<script>
	import { createEventDispatcher } from 'svelte'

	import RoseIconGrey from "./RoseIconGrey.svelte";
  import { roseTypes } from "$lib/roseTypes";
  let isGrowing = $state(false)
  let rose = $state({});
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
      }
const dispatch = createEventDispatcher()

function updateValue() {
    dispatch('updateValue', rose)
    isGrowing=false
}  

</script>


{#if !isGrowing}
  <button onclick={startGrowingRose}>
    <div  class="border-2   p-4 w-28 h-28 flex items-center justify-center rounded-lg border-dashed border-gray-300 hover:border-primary-300">
        <RoseIconGrey ></RoseIconGrey>
    
    </div>
  </button>
  {:else}
  <button onclick={updateValue}>
    <div  class="border-2  p-4 w-28 h-28 flex items-center justify-center rounded-lg border-solid border-primary-300 ">
      <img class="" width="64" height="64" src={rose.type.path} alt="">
    
    </div>
  </button>
{/if}

  