<script>
  import { onMount } from "svelte";
  let pokemon = [];
  let currentPage = 1;
  let totalPages = 0;
  let totalResults = 0;
  let next = "";
  let previous = "";
  let limit = 10;
  let offset = 0;


  onMount(() => {
    getPokemonByLimit(limit,offset);
  });


  function getPokemonByLimit(lim, off) {
    fetch(`https://pokeapi.co/api/v2/pokemon?limit=${lim}&offset=${off}`)
      .then((response) => response.json())
      .then((data) => {
        pokemon = data.results;
        currentPage = Math.floor(off / lim) + 1;
        totalPages = Math.ceil(data.count / lim);
        totalResults = data.count;
        next = data.next;
        previous = data.previous;
        offset = off;
      });
  }


</script>

<main>
  <div>
    <h1>Pokemon</h1>
    <div>
      {#each pokemon as poke}
        <div>
          <h2>{poke.name}</h2>


        </div>
      {/each}
    </div>
    <div>
      totalPages: {totalPages}
      <button
        on:click={() => {
          getPokemonByLimit(limit, offset - limit);
        }}
        disabled={currentPage === 1}
      >
        Previous
      </button>
      <button
        on:click={() => {
          getPokemonByLimit(limit, offset + limit);
        }}
        disabled={currentPage === totalPages}
      >
        Next
      </button>
    </div>
  </div>
</main>

<style>
  main {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }
</style>
