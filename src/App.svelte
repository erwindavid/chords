<script>
 
  import Scale from './lib/Scale.svelte'

  let dimension = [(document.body.clientWidth*0.85/4), (document.body.clientWidth*0.4/4)]
  let lettersOn = true
  let keyColors = 'multi'
  let mono = '#bed8fe'

  const ads = ['fiverr.png', 'shopify.png']
  let currentAd = ads[0]

  $: song = null
  $: title = 'Piano Master'

  const chords = [
    { title : 'Autumn leaves' , 
      chords : [ 'Gm7' , 'C7' , 'Fmaj7' , 'Bbmaj7' , 'Dm7' , 'G7' , 'Cmaj7' , 'Fmaj7' ] },
    { title : 'Final Fantasy 7' ,
      chords : [ 'Dbm' , 'E' , 'A' , 'F#m' ] },
  ]

  const handleClick = () => {
    song = getSong(document.querySelector('input').value)
    title = document.querySelector('input').value
    document.querySelector('input').value = ''
  }

  const toggleLetters = (e) => {
    lettersOn = e.target.checked
  }

  $: toggleColors = (e) => {
    keyColors = e.target.id
  }

  const getSong = (title) => {
    return chords.filter(e=> e.title == title).map(e=>e.chords)[0]
  }

  const componentToHex = (c) => {
    let hex = c.toString(16);
    return hex.length == 1 ? "0" + hex : hex;
  }

  const rgbToHex = (arr) => {
    return "#" + componentToHex(arr[0]) + componentToHex(arr[1]) + componentToHex(arr[2]);
  }

	$: colors = 
	{ 
    C  : "#ffc21a" ,
    Db : "#ff7700" ,
    D  : "#ff667d" ,
    Eb : "#ff2929" ,
    E  : "#4bb832" ,
    F  : "#157f48" ,
    Gb : "#37bbe6" ,
    G  : "#4463fd" ,
    Ab : "#a941e1" ,
    A  : "#8c370d" ,
    Bb : "#968a8a" ,
    B  : "#524741" ,
	}
 
  const setColor = (e) => {
    colors[e.target.id] = e.target.value
  }

  const setMono = (e) => {
    mono = e.target.value
  }


  let i=0

  const nextAdd = (table) => {
      console.log(currentAd,i)
      currentAd = table[i]
      i<table.length-1?i++:i=0
  }

  setInterval(()=>{ nextAdd(ads) },4000)
   
</script>

<div class="d-flex flex-column align-items-center justify-content-evenly w-100 bg-light p-3">

  <div class="fs-2 mb-3">
    <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" fill="currentColor" class="bi bi-music-note-beamed" viewBox="0 0 16 16" data-darkreader-inline-fill="" style="--darkreader-inline-fill: currentColor;">
      <path d="M6 13c0 1.105-1.12 2-2.5 2S1 14.105 1 13s1.12-2 2.5-2 2.5.896 2.5 2m9-2c0 1.105-1.12 2-2.5 2s-2.5-.895-2.5-2 1.12-2 2.5-2 2.5.895 2.5 2"></path>
      <path fill-rule="evenodd" d="M14 11V2h1v9zM6 3v10H5V3z"></path>
      <path d="M5 2.905a1 1 0 0 1 .9-.995l8-.8a1 1 0 0 1 1.1.995V3L5 4z"></path>
    </svg>
    {title}
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" class="bi bi-music-note" viewBox="0 0 16 16">
      <path d="M9 13c0 1.105-1.12 2-2.5 2S4 14.105 4 13s1.12-2 2.5-2 2.5.895 2.5 2"/>
      <path fill-rule="evenodd" d="M9 3v10H8V3z"/>
      <path d="M8 2.82a1 1 0 0 1 .804-.98l3-.6A1 1 0 0 1 13 2.22V4L8 5z"/>
    </svg>
  </div>

  <div class="input-group mb-3 w-50">
    <span class="input-group-text" id="basic-addon1">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-search" viewBox="0 0 16 16" data-darkreader-inline-fill="" style="--darkreader-inline-fill: currentColor;">
        <path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001q.044.06.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1 1 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0"></path>
      </svg>
    </span>
    <input id="input" type="text" class="form-control form-control-lg" value="Autumn leaves" placeholder="Input song name, chords..." aria-label="Input song name, chords..." aria-describedby="basic-addon1">
    <div class="input-group-append">
      <button type="button" class="btn btn-outline-primary btn-lg" on:click={handleClick}>Search</button>
    </div>
  </div>
  {#if song}
  <div class="d-flex flex-row align-items-center justify-content-center w-100 gap-5">
    <div class="form-check form-switch form-check-reverse">
      <input class="form-check-input" type="checkbox" id="flexSwitchCheckReverse" on:click={toggleLetters} checked>
      <label class="form-check-label" for="flexSwitchCheckReverse">Display notes : </label>
    </div>
    <div class="d-flex flex-row align-items-center gap-2">
      Keys colors :
      <div class="form-check d-flex flex-row align-items-center">
        <input class="form-check-input z-1" type="radio" name="flexRadioDefault" id="multi" on:click={toggleColors} checked>
        <div class="dropdown">
          <button class="btn btn-light dropdown-toggle text-body" type="button" id="triggerId" data-bs-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            Multi
          </button>
          <div class="container dropdown-menu ">
            {#each Object.entries(colors) as color}
              <div class="dropdown-item d-flex flex-row align-items-end">
                <label for="exampleColorInput" class="col form-label">{color[0]}</label>
                <input type="color" class="col form-control form-control-color" id={color[0]} value={color[1]} on:input={setColor}>
              </div>
            {/each}
          </div>
        </div>
      </div>
      <div class="form-check d-flex flex-row align-items-center">
        <input class="form-check-input z-1" type="radio" name="flexRadioDefault" id="mono" on:click={toggleColors}>
        <div class="dropdown">
          <button class="btn btn-light dropdown-toggle text-body" type="button" id="triggerId" data-bs-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            Mono
          </button>
          <div class="container dropdown-menu ">
              <div class="dropdown-item d-flex flex-row align-items-end">
                <input type="color" class="col form-control form-control-color" id="mono" value={mono} on:input={setMono}>
              </div>
          </div>
        </div>
      </div>
      <div class="form-check">
        <input class="form-check-input" type="radio" name="flexRadioDefault" id="none" on:click={toggleColors}>
        <label class="form-check-label" for="none">
          None
        </label>
      </div>
    </div>

  </div>
  <div class="d-flex flex-wrap ">
    {#each song as chord} 
      <Scale chord={chord} dimension={dimension} lettersOn={lettersOn} keyColors={keyColors} colors={colors} mono={mono}/>
    {/each}
  </div>
  {:else}
  No song
  {/if}
  <div class="d-flex flex-row w-100 justify-content-center p-3">
    <a class="text-center" href="#">
      <img src={"./src/img/"+currentAd} alt="ad"/>
    </a>
  </div>
</div>


<style>

  a > img {
    max-width: 70%;
  }
</style>