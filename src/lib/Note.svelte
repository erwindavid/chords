<script>

	export let name
	export let tone
	export let pressed
	export let lettersOn
	export let keyColors
	export let colors
	export let mono

	let noteWidth
	let text = '  fw-bolder'
	let full = 'full-width border' + text

	$: ghostNote = name=='X' ? ' transparent' : ''
	$: half = 'half-width border' + ghostNote + text
	$: applicableStyle = tone == "full" ? full : half
	$: colorie = (note) => {
		let ret
		if ( keyColors == 'multi' ) {
			ret =	pressed ? `${colors[note]}` : tone=="full" ? 'rgb(255,255,255)' : 'rgb(0,0,0)'
		} else if (  keyColors == 'mono' ){
			ret =	pressed ? mono : tone=="full" ? 'rgb(255,255,255)' : 'rgb(0,0,0)'
		} else {
			ret = tone=="full" ? 'rgb(255,255,255)' : 'rgb(0,0,0)'
		}
		return ret
	}
	$: letter = pressed && lettersOn ? name : ''

</script>


	<div bind:clientWidth={noteWidth} style={ tone == "half" ?"position: relative; left: "+noteWidth*0.75+"px":"" } class="full-width text-center d-flex">
		<div class={applicableStyle + " d-flex align-items-end justify-content-center"} style={"background-color: "+colorie(name)}>
			<div class="over letter">{letter}</div>
		</div>
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

.letter {
	font-size: larger;
	-webkit-text-stroke-width: 1px;
	-webkit-text-stroke-color: rgb(34, 34, 34);
	color:rgba(255, 255, 255, 0.7);
	overflow: visible;
	white-space: nowrap;
	text-overflow: ellipsis;
	padding:0;
	margin:0;
}

.over{
	position: absolute;
	z-index: 2;
} 

</style>
