<script>
    export let round = 1; // Receive round as a prop from the parent component
    export let turnCount = 0; // Receive turnCount as a prop from the parent component
    export let selectRandomStat; // Function to select a random stat
  
    $: if (turnCount === 2) {
      round++;
      turnCount = 0;
      selectRandomStat(); // Select a new random stat for the next round
    }
  </script>
  
  <div class="round-counter">
    Runde: {round}
  </div>
  
  <style>
    .round-counter {
      position: absolute;
      top: 10px;
      left: 10px;
      font-size: 20px;
      font-weight: bold;
      color: white;
      background-color: rgba(0, 0, 0, 0.5);
      padding: 5px 10px;
      border-radius: 5px;
    }
  </style>