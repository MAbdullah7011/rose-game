<script lang=ts>
	

     let Achievements = [
    {
        name: 'New Beginnings',
        desc: 'Send your first flower to the garden',
        progress: 0,
        required: 1
        
    },
    {
        name: 'Budding Garden',
        desc: 'Garden has 10 flowers',
        progress: 0,
        required: 10
    },
    {
        name: 'Expert Grower',
        desc: 'Grow 10 flowers',
        progress: 0,
        required: 10
    },
    {
        name: 'Flourishing Garden',
        desc: 'Garden has 100 flowers',
        progress: 0,
        required: 10
    }

    ]
</script>


<div class="card preset-filled-surface-50-950 border-[1px] border-surface-200-800 place-items-center h-screen p-20 space-y-10">
    
    
    {#each Achievements as achievement}
    
        <div class="max-w-fit space-y-1">
            <div >
                <h6 class="h6 ">{achievement.name}</h6>
                <p >{achievement.desc}</p>
            </div>
    
            <div class="flex items-center gap-2">
                <progress class="progress w-56  md:w-[500px] " value="{achievement.progress}" max="{achievement.required}"></progress>
                {achievement.progress/achievement.required*100}%
            </div>
        </div>
    
    {/each}
    
    
</div>