<script>
  import Pushup from "./Pushup.svelte";
  import Situp from "./Situp.svelte";

  let mode = "situp";

  function changeMode(input) {
    mode = input;
  }
</script>

<main>
  <button on:click={() => changeMode("situp")}>Situp</button>
  <button on:click={() => changeMode("pushup")}>Pushup</button>

  <div>
    {#if mode === "pushup"}
      <Pushup />
    {:else}
      <Situp />
    {/if}
  </div>
</main>

<style>
  main {
    text-align: center;
    padding: 2em;
  }

  button {
    padding: 10px 20px;
    margin: 5px;
    font-size: 16px;
  }
</style>
