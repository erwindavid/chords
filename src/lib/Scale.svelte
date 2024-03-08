<script>

  import Note from './Note.svelte'

  export let chord

  export let dimension

  let tones = ['C','D','E','F','G','A','B']

  let halfTones = ['Db','Eb','X','Gb','Ab','Bb','X']

  //let halfTones = [['Db','C#'],['Eb','D#'],'X',['Gb','F#'],['Ab','G#'],['Bb','A#'],'X']

  const chords = [
	{chord:'Gm7', notes:['G','Bb','D','F']},
	{chord:'C7', notes:['C','E','G','Bb']},
	{chord:'Fmaj7', notes:['F','A','C','E']},
	{chord:'Bbmaj7', notes:['Bb','D','F','A']},
	{chord:'Dm7', notes:['D','F','A','C']},
	{chord:'G7', notes:['G','B','D','F']},
	{chord:'Cmaj7', notes:['C','E','G','B']},
	{chord:'Fmaj7', notes:['F','A','C','E']}
	]
	
  const notes = chords.find(e=>e.chord == chord).notes


</script>

<div class="border p-3">
	<h5 class="text-center">{chord}</h5>
	<div class="text-center">{notes.join(' - ')}</div>
	<div class="d-flex flex-column par" style={"width:"+dimension+"px;height:"+dimension+"px"}>
		<div class="d-flex flex-row full-width half-height stack over" >
		{#each halfTones as note}
			<Note name={note} pressed={notes.includes(note)} tone="half"/>
		{/each}
		</div>
		<div class="d-flex flex-row full-width full-height stack">
		{#each tones as note}
			<Note name={note} pressed={notes.includes(note)} tone="full"/>
		{/each}
		</div>
	</div>
</div>
<style>

.par {
	position:relative;
}
.stack {
	position: absolute;
	top:0;
	left:0
}
.over {
	z-index: 10;
}
.half-height {
	height: 60%
}
.full-width {
	width: 100%;
}
.full-height {
	height:100%
}

</style>