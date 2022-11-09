<script>
  // @ts-nocheck

  import { onMount } from "svelte";

  let words = [];
  let formatProposition = [];
  let andOrOr = "and";

  onMount(async () => {
    const response = await fetch(
      "https://random-word-api.herokuapp.com/word?number=10"
    );
    words = await response.json();
  });

  const addToFormula = (word) => {
    if (formatProposition.length > 0) {
      formatProposition.push(andOrOr);
    }
    formatProposition.push(word);
    formatProposition = formatProposition;
  };

  const addParenthesisBetweenWords = () => {
    formatProposition = ["(", ...formatProposition, ")"];
  };

  const addParenthesisOpen = () => {
    formatProposition = ["(", ...formatProposition];
  };

  const addParenthesisClose = () => {
    formatProposition = [...formatProposition, ")"];
  };

  const deleteLastWord = () => {
    formatProposition = formatProposition.slice(0, -1);
  };

  const reset = () => {
    formatProposition = [];
  };

</script>

<main>
  <h1>Random Words</h1>
  <div class="grid">
    {#each words as word}
      <div class="card" on:click={addToFormula(word)}>{word}</div>
    {/each}
  </div>
  <div class="grid">
    <input type="radio" bind:group={andOrOr} id="and" value="and" />
    <label for="and">AND</label>
  </div>
  <div class="grid">
    <input type="radio" bind:group={andOrOr} id="or" value="or" />
    <label for="or">OR</label>
  </div>
  <div class="grid">
    <button on:click={addParenthesisBetweenWords}>(...)</button>
    <button on:click={addParenthesisOpen}>(</button>
    <button on:click={addParenthesisClose}>)</button>
    <button on:click={deleteLastWord}>DEL</button>
    <button on:click={reset}>reset</button>
  </div>
<h1>Phrase</h1>
  <div class="mini-grid">
    {#each formatProposition as word}
      <div class="word-card" style="color: {word == "(" || word == ")" ? 'red': 'green'}">{word}</div>
    {/each}
  </div>
</main>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Mulish&display=swap");

  * {
    font-family: "Mulish", sans-serif;
  }

  .mini-grid {
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    grid-gap: 1rem;
  }

  button {
    border: none;
    background-color: rgb(37, 137, 104);
    color: white;
    padding: 0.5rem 0.3rem;
    cursor: pointer;
  }

  button:hover {
    background-color: green;
  }

  .word-card {
    background-color: rgb(177, 255, 229);
    padding: 0.5rem 0.3rem;
    cursor: pointer;
  }

  .card {
    border: 2px solid rgb(70, 212, 184);
    padding: 0.5rem;
    margin: 0.5rem;
    cursor: pointer;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    grid-gap: 1rem;
  }
</style>
