<script lang=ts>
    let achievements:String[] = ['Achievement 1', 'Achievement 2', 'Achievement 3', 'Achievement 4', 'Achievement 5'];
</script>


<div class="card preset-filled-surface-50-950 border-[1px] border-surface-200-800 w-full max-w-full h-screen p-4 text-center">
    <div class="space-y-10 p-3 ">
        {#each achievements as achievement}
            
            <div class="flex gap-10 p-2 items-center justify-center">
                <div>
                    <p>{achievement}</p>
                    description
                </div>
                <progress class="progress max-w-lg" value="50" max="100"></progress>
            </div>
            
            
        {/each}
    </div>
</div>