<script>
  import { onDestroy } from "svelte";
  import workoutData from "./workoutData.json";

  let userInput = 2; // default input value to 2 minutes
  let timeLeft = 120; // default to 2 minutes in seconds
  let interval;

  function speakTimeLeft() {
    let message = `${timeLeft} seconds left`;
    let utterance = new SpeechSynthesisUtterance(message);
    window.speechSynthesis.speak(utterance);
  }

  function startCountdown() {
    timeLeft = userInput * 60; // convert user input to seconds
    clearInterval(interval); // clear any existing intervals

    interval = setInterval(() => {
      timeLeft -= 1;

      if (timeLeft % 30 === 0 && timeLeft !== 0) {
        speakTimeLeft();
      }

      if (timeLeft <= 0) {
        clearInterval(interval);
        let message = workoutData.defaults.message;
        let utterance = new SpeechSynthesisUtterance(message);
        window.speechSynthesis.speak(utterance);
      }
    }, 1000);
  }

  function stopCountdown() {
    clearInterval(interval);
  }

  function reset() {
    stopCountdown();
    timeLeft = userInput * 60;
  }

  // Clear interval on component destroy
  onDestroy(() => {
    clearInterval(interval);
  });
</script>

<main>
  <h1>Pushups</h1>
  <div>
    Set timer for:
    <input type="number" bind:value={userInput} min="1" step="1" /> minutes
  </div>
  <p>
    Time left: {Math.floor(timeLeft / 60)}:{(timeLeft % 60)
      .toString()
      .padStart(2, "0")}
  </p>
  <button on:click={startCountdown}>Start</button>
  <button on:click={stopCountdown}>Stop</button>
  <button on:click={reset}>Reset</button>
</main>

<style>
  main {
    text-align: center;
    padding: 2em;
  }
</style>
