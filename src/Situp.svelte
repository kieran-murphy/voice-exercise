<script>
  import { onDestroy } from "svelte";
  import workoutData from "./workoutData.json";

  let targetNumber = workoutData.defaults.situp;
  let currentCount = 0;
  let interval;
  let shouldContinue = true;

  function sleep(ms) {
    return new Promise((resolve) => setTimeout(resolve, ms));
  }

  function speak(number) {
    let utterance = new SpeechSynthesisUtterance(number.toString());
    window.speechSynthesis.speak(utterance);
  }

  async function startCountUp() {
    // stopCounting(); // clear any existing intervals
    currentCount = 0; // reset count

    speak("workout will start in 5 seconds");
    await sleep(5000);

    while (currentCount < targetNumber && shouldContinue) {
      await sleep(500);
      speak("raise");
      await sleep(1500);
      speak("down");
      await sleep(500);
      currentCount += 1;
      speak(currentCount);
    }

    // Speak the final message after the loop completes
    setTimeout(() => speak(workoutData.defaults.message), 2000);
  }

  function stopCounting() {
    shouldContinue = false;
  }

  function reset() {
    stopCounting(); // stop any ongoing count
    currentCount = 0; // reset the count
    shouldContinue = true; // reset the control variable
  }

  // Clear interval on component destroy
  onDestroy(() => {
    stopCounting();
  });
</script>

<main>
  <h1>{currentCount}</h1>
  <input
    type="number"
    bind:value={targetNumber}
    placeholder="Enter a target number"
  />
  <button on:click={startCountUp}>Start</button>
  <button on:click={stopCounting}>Stop</button>
  <button on:click={reset}>Reset</button>
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

  input,
  button {
    padding: 10px;
    font-size: 1.2em;
    margin-right: 10px;
  }
</style>
