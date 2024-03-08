<script>

	export let name
	export let tone
	export let pressed

	console.log(pressed, name, tone)

	let noteWidth
	let full = 'full-width text-black border'
	let ghostNote = name=='X' ? ' transparent' : ''
	let half = 'half-width text-white border ' + ghostNote 
	let applicableStyle = tone == "full" ? full : half
	let background = pressed ? ' bg-danger' : tone=="full" ? ' bg-white' : ' bg-black'
	
</script>

<div bind:clientWidth={noteWidth} style={ tone == "half" ?"position: relative; left: "+noteWidth*0.75+"px":""} class={"full-width text-center d-flex"}>
	<div class={applicableStyle + background + " d-flex align-items-center justify-content-center"}></div>
</div>

<style>
.transparent {
	opacity: 0;
}

.full-width {
	width: 100%;
}

.half-width {
	width: 50%
}

</style>
