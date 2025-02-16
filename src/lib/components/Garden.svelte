<script>
  import * as Popover from "$lib/components/ui/popover/index.js";

	

  export let gardenCollection;

</script>



<div class="flex flex-wrap gap-3  border min-h-96 rounded-lg p-4 relative bg-gradient-to-br from-green-50 to-rose-50" >
  {#each gardenCollection as rose (rose.id)}
    
    {#if rose.note.length > 0}
    
      <div class="flex items-center h-fit min-h-[64px] ml-[52px]  break-all p-1 max-w-28 max-h-[64px] bg-surface-50 border hover:shadow-inner border-surface-100 rounded-sm text-xs shadow-sm relative">
        <p>{rose.note}</p>
        <img  class="h-fit  absolute -top-0 -left-12" width="64" height="64" src={rose.type.path} alt="">
      </div>
    
      {:else}
      <img  class="h-fit hover:border" width="64" height="64" src={rose.type.path} alt="">
    {/if}
  {/each}
  
    
  <div class="absolute top-3 right-3">
    <Popover.Root>
      <Popover.Trigger>
        <span class="badge-icon border-surface-300 border bg-surface-50 text-surface-600 ">
          i
        </span>
      </Popover.Trigger>
      <Popover.Content side="left" sideOffset={-24} align="start"  avoidCollisions={false}>
        <p>Total Roses:</p>
        <p>Sent by you:</p>
        <p>Sent by them:</p>
      </Popover.Content>
    </Popover.Root>
  </div>   
  
</div>