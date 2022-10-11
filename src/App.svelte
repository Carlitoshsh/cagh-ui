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
  let paginationSpaces = 4;

  onMount(() => {
    getPokemonByLimit(limit, offset);
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

      <h2>Pagination</h2>
      <div>
        <button
          on:click={() => {
            getPokemonByLimit(limit, 0);
          }}
          disabled={currentPage === 1}
        >
          First
        </button>
        <button
          on:click={() => {
            getPokemonByLimit(limit, (totalPages - 1) * limit);
          }}
          disabled={currentPage === totalPages}
        >
          Last
        </button>
      </div>
      {{ currentPage }} of {{ totalPages }}
      <div>
        {#each Array.from({ length: paginationSpaces }) as _, i}
          {#if currentPage - paginationSpaces + i > 0}
            <button
              on:click={() => {
                getPokemonByLimit(
                  limit,
                  (currentPage - paginationSpaces + i - 1) * limit
                );
              }}
              disabled={currentPage === currentPage - paginationSpaces + i}
            >
              {currentPage - paginationSpaces + i}
            </button>
          {:else if currentPage + i < totalPages && currentPage + i < paginationSpaces}
            <button
              on:click={() => {
                getPokemonByLimit(limit, (currentPage + i - 1) * limit);
              }}
              disabled={currentPage === currentPage + i}
            >
              {currentPage + i}
            </button>
          {/if}
        {/each}
      </div>
    </div>
  </div>
</main>

<a href="https://alistapart.com/article/web-typography-tables/">Responsive tables</a>

<style>
  main {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .flex-space-between {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .flex-center {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    grid-gap: 1rem;
  }

  .grid-item {
    /* background-color: #f1f1f1; */
    padding: 1rem;
    border-radius: 0.25rem;
  }

/* table with data header */
  table {
    border-collapse: collapse;
    width: 100%;
  }

  th,
  td {
    text-align: left;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #f2f2f2;
  }

  th {
    background-color: #4caf50;
    color: white;
  }

  .pagination {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
