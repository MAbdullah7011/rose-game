<script>
  
  import { Popover } from "bits-ui";
  let {gardenCollection} = $props();
  let total = $derived(gardenCollection.length)

</script>



<div class="flex flex-wrap gap-3 content-start border min-h-96 rounded-lg p-4 relative  bg-gradient-to-br from-green-50 to-rose-50" >
  {#each gardenCollection as rose (rose.id)}
    
    
    <Popover.Root >
      <Popover.Trigger>
        <img  class="h-fit border border-transparent hover:drop-shadow-lg hover:border-0" width="64" height="64" src={rose.type.path} alt="">
      </Popover.Trigger>
      <Popover.Content  side="top" sideOffset={10} align="start" class="w-56 bg-card border rounded-md p-3 drop-shadow-lg" avoidCollisions={false}>
        <div >
          <div>{rose.type.name}</div>
          <div class="text-surface-400 font-bold text-sm">sent by you</div>
          <div class="text-sm text-surface-500 "> {rose.note.length>0?rose.note:"no note"}</div>
          
        </div>
        <Popover.Arrow class="text-card"></Popover.Arrow>
      </Popover.Content>
    </Popover.Root>
      
      
    
  {/each}
  
    
  <div class="absolute top-3 right-3">
    <Popover.Root>
      <Popover.Trigger>
        <span class="badge-icon border-surface-300 border bg-surface-50 text-surface-600 ">
          i
        </span>
      </Popover.Trigger>
      <Popover.Content class="w-fit leading-tight text-base tracking-tight  bg-card border rounded-md p-3 drop-shadow-lg" side="left" sideOffset={-24} align="start"  avoidCollisions={false}>
        <p>Total Roses: {total}</p>
        <p>Sent by you: {total}</p>
        <p>Sent by them: 0</p>
      </Popover.Content>
    </Popover.Root>
  </div>   
  
</div>