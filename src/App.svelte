<script>
 
  import Scale from './lib/Scale.svelte'

  let chords = ['Gm7','C7','Fmaj7','Bbmaj7','Dm7','G7','Cmaj7','Fmaj7']

  let dimension = document.body.clientWidth / (chords.length+1)

  console.log(dimension)

</script>

<div class="d-flex flex-wrap">
	{#each chords as chord} 
		<Scale chord={chord} dimension={dimension}/>
	{/each}
</div>