<script>
  import { createEventDispatcher } from "svelte";
  /**
   * Is this the principal call to action on the page?
   */
  export let primary = false;

  /**
   * What background color to use
   */
  export let backgroundColor;
  /**
   * How large should the button be?
   */
  export let size = "medium";
  /**
   * Button contents
   */
  export let label = "";

  let mode = primary ? "primary" : "secondary";

  let style = backgroundColor ? `background-color: ${backgroundColor}` : "";

  const dispatch = createEventDispatcher();

  /**
   * Optional click handler
   */
  function onClick(event) {
    dispatch("click", event);
  }
</script>

<button
  type="button"
  class={[`button-${size}`, mode].join(" ")}
  {style}
  on:click={onClick}
>
  {label}
</button>

<style>
  button {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    border: none;
    border-radius: 0.5rem;
    padding: 8px 16px;
    font-size: 1rem;
    cursor: pointer;
  }

  button.primary {
    background: #134975;
    color: #fff;
  }

  button.secondary {
    background: #ff3e00;
    color: #fff;
  }

  button.button-small {
    font-size: 0.875rem;
    padding: 4px 8px;
  }

  button.button-large {
    font-size: 1.25rem;
    padding: 12px 24px;
  }

  button.button-medium {
    font-size: 1rem;
    padding: 8px 16px;
  }

  button:focus {
    outline: 1px solid #fff;
    outline-offset: -4px;
  }
</style>
