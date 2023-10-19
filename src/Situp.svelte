<script>
  let number = 0;
</script>

<main>
  <h1>{number}</h1>
  <input type="number" bind:value={number} placeholder="Enter a number" />
</main>

<style>
  main {
    text-align: center;
    padding: 2em;
  }

  h1 {
    font-size: 2em;
    margin-bottom: 20px;
  }

  input {
    padding: 10px;
    font-size: 1.2em;
  }
</style>
