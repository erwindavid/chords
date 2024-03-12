<script>

  import Note from './Note.svelte'

  export let chord
  export let dimension
  export let lettersOn
  export let keyColors
  export let colors
  export let mono

  const scale =  ['C','Db','D','Eb','E','X','F','Gb','G','Ab','A','Bb','B','X','C','Db','D','Eb','E','X','F','Gb','G','Ab','A','Bb','B','X']
 
  const chords = [
	{ chord : 'Gm7' , notes : [ 'G' , 'Bb' , 'D' , 'F' ] } , 
	{ chord : 'C7' , notes : [ 'C' , 'E' , 'G' , 'Bb' ] } , 
	{ chord : 'Fmaj7' , notes : [ 'F' , 'A' , 'C' , 'E' ] } , 
	{ chord : 'Bbmaj7' , notes : [ 'Bb' , 'D' , 'F' , 'A' ] } , 
	{ chord : 'Dm7' , notes : [ 'D' , 'F' , 'A' , 'C' ] } , 
	{ chord : 'G7' , notes : [ 'G' , 'B' , 'D' , 'F' ] } , 
	{ chord : 'Cmaj7' , notes : [ 'C' , 'E' , 'G' , 'B' ] } , 
	{ chord : 'Fmaj7' , notes : [ 'F' , 'A' , 'C' , 'E' ] },
	{ chord : 'A' , notes : [ 'A' , 'Db' , 'E' ] },
	{ chord : 'E' , notes : [ 'E' , 'Ab' , 'B' ] },
	{ chord : 'Dbm' , notes : [ 'Db' , 'E' , 'Ab' ] },
	{ chord : 'F#m' , notes : [ 'Gb' , 'A' , 'Db' ] },
	]

  const getSeq = ( chord, tones ) => {
	let c = 0
	return chord.reduce( ( acc, cur ) => {
		let pos = tones.slice( c ).findIndex( e => e == cur )
		if ( pos > -1 ) {
			acc.push( c + pos )
			c += pos
		}
		return acc

	}, [] )
  } 

  const tones = scale.filter(e=> scale.indexOf(e)%2==0 ).map(e=>e)
  const halfTones = scale.filter(e=> scale.indexOf(e)%2==1 ).map(e=>e)
    	
  $: notes = chords.find(e=>e.chord == chord).notes
  $: fullScalePressed = getSeq(notes, scale)
  $: tonesPressed = fullScalePressed.filter(e=> e%2==0 ).map(e=> e/2)
  $: halfTonesPressed = fullScalePressed.filter(e=> e%2==1 ).map(e=> Math.floor(e/2))

</script>

<div class="border p-3">
	<h5 class="text-center">{chord}</h5>
	{#if lettersOn}
		<div class="text-center">{notes.join(' - ')}</div>
	{/if}
	<div class="d-flex flex-column par" style={"width:"+dimension[0]+"px;height:"+dimension[1]+"px"}>
		<div class="d-flex flex-row full-width half-height stack over" >
		{#each halfTones as note, index}
			<Note name={note} pressed={halfTonesPressed.includes(index)} tone="half" lettersOn={lettersOn} keyColors={keyColors} colors={colors} mono={mono}/>
		{/each}
		</div>
		<div class="d-flex flex-row full-width full-height stack">
		{#each tones as note, index}
			<Note name={note} pressed={tonesPressed.includes(index)} tone="full" lettersOn={lettersOn} keyColors={keyColors} colors={colors} mono={mono}/>
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