<script>
  export let node;
</script>

<details>
  <summary>{node.name}</summary>
  <p>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Repellat sint,
    accusamus consectetur doloribus numquam dolorum mollitia tempora. Doloribus
    incidunt recusandae ipsa maxime, mollitia dolore odio. Pariatur deserunt
    impedit nesciunt molestiae.
  </p>
  {#each node.children as child}
    <svelte:self node={child} />
  {/each}
</details>

<style>
  details {
    margin: 1rem;
    border: 1px solid;
    padding: 1rem;
    border-radius: 0.5rem;
  }
  summary {
    cursor: pointer;
    background-color: black;
    color: white;
    padding: 0.5rem;    
  }
</style>
