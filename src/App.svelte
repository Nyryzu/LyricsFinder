<script>
import {onMount} from 'svelte' ;
import { element } from 'svelte/internal';

var userInput ; 
var errorBox ;
var lyricsField
const apiURL = `https://api.lyrics.ovh` ;


onMount(() => {
        errorBox;
        userInput;
        lyricsField;
});

//--------------------------------------------------------------------------------
async function search() {
    let searchValue = userInput.value;
        
    if (!searchValue.trim()){
        errorBox.style.display = 'block' ;
        setTimeout(() => {
            errorBox.style.display = 'none' ; 
			 ;
        }, 2500); 
		return;
    }		

    const searchResult = await fetch(`${apiURL}/suggest/${searchValue}`)
    const data = await searchResult.json();

    showData(data);
   
}

//display final result in DOM
function showData(data){
    
	lyricsField.innerHTML = `
    <ol class="song-list">
      ${data.data
        .map(song=> `<li>
                    <div>
                        <strong>${song.artist.name}</strong> -${song.title} 
                    </div>
                    <button on:click="${click()}" data-artist="${song.artist.name}" data-songtitle="${song.title}" > get lyrics </button>
					</li>`
        )
        .join('')}
    </ol>
  `;
  
}
	
//event listener in get lyrics button

async function click() {

    await lyricsField.addEventListener('click', e=>{
        const clickedElement = e.target ;
        //checking clicked elemet is button or not
        if (clickedElement.tagName === 'BUTTON'){
            const artist = clickedElement.getAttribute('data-artist');
            const songTitle = clickedElement.getAttribute('data-songtitle');
            getLyrics(artist, songTitle)
        }
    })
}

// Get lyrics for song
async function getLyrics(artist, songTitle) {

    const res = await fetch(`${apiURL}/v1/${artist}/${songTitle}`);
    const data = await res.json() ;
    const lyrics = data.lyrics.replace(/(\r\n|\r|\n)/g, '<br>');
    lyricsField.innerHTML = ` 
    <h4 style="margin-bottom:30px; position: center"><strong>${artist}</strong> - ${songTitle}</h4><ul>
    <div data-artist="${artist}" data-songtitle="${songTitle}"></div>
    <p style="margin-top:20px;">${lyrics}</p>
`    
}
async function getVideo(artist, songTitle) {

const res = await fetch(`https://www.googleapis.com/youtube/v3/search/${artist}/${songTitle}`);
const data = await res.json() ;


}

</script>


<!------------------------------------------------------------------------------------------------->

<main>
	
	<head>
		<meta charset="UTF-8">
		<meta http-equiv="X-UA-Compatible" content="IE=edge">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>Lyric Finder</title>
			   
	</head>
		
		<body class="bg-light">
			<div class="container px-5 py-5">
				<h2 class="text-center pb-3">Lyrics Finder</h2>
				
				<div class="row">
					<div align="center">
						<input style="width: 500px" type="text" class="form-control input-artist" placeholder="Enter artist name or song title..." bind:this={userInput}>
					</div>
				</div>
				<div class="text-center">
					<button on:click="{search}" class="btn btn-outline-success btn-lg mt-4 find-btn">Find Lyrics!</button>
				</div>
			  
			</div>
		
			<div class="text-center">
				<div class="card text-white bg-info mb-3 mt-5 text-center">
					<div class="card-header">Lyrics Box</div>
					<div class="card-body" bind:this={lyricsField} >
						<h5 class="card-title song-desc" >Your quick lyrics finder</h5>
						<p class="card-text lyrics-text" > Simply type in the artist name and the song name and get the lyrics 😊</p>
					</div>
				</div>
			</div>
		
			<div class="alert alert-danger error-box " role="alert" bind:this={errorBox}>
				<h4 class="alert-heading text-center">Error!</h4>
				<p class="text-center">Please check your input</p>
			</div>
            <footer style="padding:left:0%;">Made by Nyryzu(EEDCVP) 2021</footer>
		</body>
</main>


<!----------------------------------------------------------------------------------------------->


<style>
	main {
		text-align:center;
		padding: 1em;
		max-width: 600px;
		margin: 0 auto;
    }
    .container{
        position:relative;
    }

    .error-box {
        display: none;
        position: top;
        top: 50% ;
        left: 50%;
        transform: translate(-50%,-50%);
        box-shadow: 3px 4px 5px rgba(0, 0, 0, .2);
    }
    
</style>