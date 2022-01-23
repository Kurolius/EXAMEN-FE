<script>
import {onMount} from 'svelte';
    let allmovies = {}
    let movies = []
    let pages = 3

    const getValues =()=> {
        let url = window.location.search;
        let p =url.split('=')[1]
		return p
    }
    onMount(async () => {
        try{
            const response = (getValues()) ? await fetch('http://localhost:3000/movies?take=10&skip='+getValues()) : await fetch('http://localhost:3000/movies')
            allmovies = await response.json()
            movies=allmovies.data
        }catch(Err){
            console.log("error fetching data")
        }
    } )
</script>

<main align="center">
    <div align="center">
        <div align="center" class="uk-width-5-6 " uk-grid="parallax : 50">
            {#each movies as movie (movie.id)}
            <div class="uk-width-1-3">
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                        <h5 class="card-title">{movie.title}</h5>
                        <h6 class="card-subtitle mb-2 text-muted">
                            {movie.year}
                        </h6>
                        <h6 class="card-subtitle mb-2 text-muted">
                            {#each movie.genres as genre}
                                {genre}/
                            {/each}
                        </h6>
                        <p class="card-text">
                            {#if movie.plot}
                                {movie.plot}
                            {/if}
                        </p>
                        <h6 class="card-subtitle mb-2 text-muted">Rating</h6>
                        <h6 class="card-subtitle mb-2 text-muted">imdb : {movie.imdb.rating}</h6>
                        <h6 class="card-subtitle mb-2 text-muted">Tomatoes : {movie.tomatoes.viewer.rating}</h6>
                    </div>
                </div>
            </div>
            {:else }
                <div uk-spinner></div> Loading...
            {/each}
        </div>
        <nav aria-label="...">
        <ul class="pagination pagination-lg">
            {#each {length: 2353} as _, i}
                {#if i+1 != getValues()}
                    <li class="page-item"><a class="page-link" href="?page={i+1}">{i+1}</a></li>
                {:else}
                    <li class="page-item active" aria-current="page">
                        <span class="page-link">{getValues()}</span>
                    </li>
                {/if}
            {/each}
            
        </ul>
        </nav>
        
    </div>
</main>

<style>
	
</style>