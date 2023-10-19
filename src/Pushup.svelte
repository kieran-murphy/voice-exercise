<script>
  import { onDestroy } from "svelte";

  let userInput = 2; // default input value to 2 minutes
  let timeLeft = 120; // default to 2 minutes in seconds
  let interval;

  function speakTimeLeft() {
    let message = `${timeLeft} seconds left`;

    // Create a new SpeechSynthesisUtterance and set its text
    let utterance = new SpeechSynthesisUtterance(message);

    // Speak the message
    window.speechSynthesis.speak(utterance);
  }

  function startCountdown() {
    timeLeft = userInput * 60; // convert user input to seconds

    // Ensure that clicking the button multiple times doesn't create multiple intervals.
    clearInterval(interval);

    interval = setInterval(() => {
      timeLeft -= 1;

      // If the time left is a multiple of 30 seconds, announce it.
      if (timeLeft % 30 === 0 && timeLeft !== 0) {
        speakTimeLeft();
      }

      if (timeLeft <= 0) {
        clearInterval(interval);
        let message = "Time is up";
        // let message = "floopy woopy flumberjacks";
        let utterance = new SpeechSynthesisUtterance(message);
        window.speechSynthesis.speak(utterance);
      }
    }, 1000);
  }

  // Clear interval on component destroy
  onDestroy(() => {
    clearInterval(interval);
  });
</script>

<main>
  <h1>Countdown Timer</h1>
  <div>
    Set timer for:
    <input type="number" bind:value={userInput} min="1" step="1" /> minutes
  </div>
  <p>
    Time left: {Math.floor(timeLeft / 60)}:{(timeLeft % 60)
      .toString()
      .padStart(2, "0")}
  </p>
  <button on:click={startCountdown}>Start Countdown</button>
</main>

<style>
  main {
    text-align: center;
    padding: 2em;
  }
</style>
