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


<div class="card preset-filled-surface-50-950 border-[1px] border-surface-200-800 w-full max-w-full h-screen p-20 space-y-10">
    
    
    {#each Achievements as achievement}
    
        <div>
            <div >
                <h6 class="h6">{achievement.name}</h6>
                <p >{achievement.desc}</p>
            </div>
    
            <span>
                <progress class="progress max-w-lg " value="{achievement.progress}" max="{achievement.required}"></progress>
                {achievement.progress/achievement.required*100}%
            </span>
        </div>
    
    {/each}
    
    
</div>