<script>
  import { onMount, onDestroy } from 'svelte';
  import data from '../assets/animaldata.js'; 
  import Card from '../components/Card.svelte'; 
  import Rounds from '../components/Rounds.svelte'; 
  import '../styles/game.css'; 

  let player1Profile = 'images/Profile/profile.jpg'; 
  let player2Profile = localStorage.getItem('player2ProfileImage') || 'images/Profile/profile2.jpg'; 
  let player1Name = localStorage.getItem('player1Name') || "Player 1";
  let player2Name = localStorage.getItem('player2Name') || "Player 2";
  let player1Cards = [];
  let player2Cards = [];
  let selectedCardName = ''; 
  let computerSelectedCard = '';  
  let message = ''; 
  let gameOver = false;
  let playedCard = null; // Add this line
  let fadeOut = false; // Add this line
  let player1Won = false; // Add this line
  let isTie = false; // Add this line
  let player1RoundsWon = 0;
  let player2RoundsWon = 0;
  let totalRounds = 10; // Define the total number of rounds for the game
  let rounds = [];
  let player1Stat = 65; // Placeholder for player 1's stat
  let player2Stat = 56; // Placeholder for player 2's stat

  let isPlayer1Turn = true; // Determines whose turn it is
  let startingPlayer1 = true; // Tracks which player starts the game
  let round = 1; // Round counter
  let turnCount = 0; // Track the number of turns in the current round
  let selectedStat = ''; // Track the selected stat for the current round

  const stats = ['max_weight', 'max_length', 'max_age', 'top_speed', 'litter_size', 'deaths'];
  const statIcons = {
    max_weight: 'game_icons/weight.png',
    max_length: 'game_icons/length.png',
    max_age: 'game_icons/age.png',
    top_speed: 'game_icons/speed.png',
    litter_size: 'game_icons/offspring.png',
    deaths: 'game_icons/death.png'
  };

  onMount(() => {
    document.body.classList.add('game-page');
    startNewGame();
  });

  onDestroy(() => {
    document.body.classList.remove('game-page');
  });

  function startNewGame() {
    const shuffledData = data.sort(() => 0.5 - Math.random());
    player1Cards = shuffledData.slice(0, 5);
    player2Cards = shuffledData.slice(5, 10);
    isPlayer1Turn = startingPlayer1; // Set starting player
    window.scrollTo(0, 0);
    round = 1; // Reset round counter
    turnCount = 0; // Reset turn counter
    gameOver = false;
    message = ''; 
    fadeOut = false; // Reset fadeOut
    player1Won = false; // Reset player1Won
    isTie = false; // Reset isTie
    selectedCardName = ''; // Clear selected card
    computerSelectedCard = ''; // Clear computer selected card
    selectRandomStat(); // Select a random stat for the first round

    if (!isPlayer1Turn) {
      setTimeout(computerSelectCard, 1000);
    }
  }

  function selectRandomStat() {
    selectedStat = stats[Math.floor(Math.random() * stats.length)];
    console.log(`Round ${round}: Selected stat is ${selectedStat}`);
  }

  function processTurn(playerCard) {
    const cleanStat = value => parseFloat(value.toString().replace(',', '.'));

    console.log(
      `${isPlayer1Turn ? player1Name : player2Name} played card: ${playerCard.name}, ${selectedStat}: ${cleanStat(playerCard[selectedStat])}`
    );

    if (isPlayer1Turn) {
      selectedCardName = playerCard.name;
      player1Cards = player1Cards.filter(c => c.name !== selectedCardName);
    } else {
      computerSelectedCard = playerCard.name;
      player2Cards = player2Cards.filter(c => c.name !== computerSelectedCard);
    }

    if (checkGameOver()) return;

    turnCount++;

    if (turnCount === 2) {
      // Compare stats and announce the round winner
      const player1Card = data.find(c => c.name === selectedCardName);
      const player2Card = data.find(c => c.name === computerSelectedCard);

      if (player1Card && player2Card) {
        const player1Stat = cleanStat(player1Card[selectedStat]);
        const player2Stat = cleanStat(player2Card[selectedStat]);

        if (player1Stat > player2Stat) {
          showMessage(`Du gewinnst diese Runde! ${player1Stat} vs ${player2Stat}`);
          player1Won = true; // Set player1Won to true
          isTie = false; // Set isTie to false
          player1RoundsWon++; // Increment player1RoundsWon
          console.log(`${player1Name} wins the round!`);
        } else if (player1Stat < player2Stat) {
          showMessage(`${player2Name} gewinnt diese Runde! ${player1Stat} vs ${player2Stat}`);
          player1Won = false; // Set player1Won to false
          isTie = false; // Set isTie to false
          player2RoundsWon++; // Increment player2RoundsWon
          console.log(`${player2Name} wins the round!`);
        } else {
          showMessage(`Unentschieden! ${player1Stat} vs ${player2Stat}`);
          player1Won = false; // Set player1Won to false
          isTie = true; // Set isTie to true
          console.log(`The round is a tie!`);
        }
      }

      // Trigger fade-out effect
      setTimeout(() => {
        fadeOut = true;
        setTimeout(() => {
          fadeOut = false;
          selectedCardName = '';
          computerSelectedCard = '';
          // Trigger Player 2's turn after the animation is complete
          if (!isPlayer1Turn) {
            setTimeout(computerSelectCard, 1000);
          }
        }, 1000);
      }, 2000);

      // Reset turn count and move to the next round
      turnCount = 0;
      round++;
      selectRandomStat();
      checkGameOver(); // Check if the game is over after each round
    } else {
      // Switch turns
      isPlayer1Turn = !isPlayer1Turn;

      // Trigger Player 2's turn if it's now their turn
      if (!isPlayer1Turn) {
        setTimeout(computerSelectCard, 1000);
      }
    }
    playedCard = playerCard; // Add this line
  }

  function updateSelectedCard(card) {
    if (!isPlayer1Turn) return; // Prevent Player 1 from playing out of turn
    playedCard = card; // Add this line
    processTurn(card);
  }

  function computerSelectCard() {
    const randomIndex = Math.floor(Math.random() * player2Cards.length);
    const selectedCard = player2Cards[randomIndex];
    playedCard = selectedCard; // Add this line
    processTurn(selectedCard);
  }

  function checkGameOver() {
    if (player1Cards.length === 0 && player2Cards.length === 0) {
      gameOver = true;
      message = `Du gewinnst diese Runde! ${player1Stat} vs ${player2Stat}`;
      console.log(`Game Over: ${gameOver}, Message: ${message}`); // Add this line

      return true;
    }
    if (round >= totalRounds) {
      endGame();
    }
    return false;
  }

  function restartGame() {
    startingPlayer1 = !startingPlayer1; // Alternate starting player
    startNewGame();
  }

  function showMessage(newMessage) {
    message = newMessage;
    setTimeout(() => {
      message = '';
    }, 4000);
  }

  $: {
    // Trigger bounce animation when selectedStat changes
    const statIconElement = document.querySelector('.selected-stat-icon img');
    if (statIconElement) {
      statIconElement.classList.remove('bounce');
      void statIconElement.offsetWidth; // Trigger reflow to restart the animation
      statIconElement.classList.add('bounce');
    }
  }

  $: winner = player1RoundsWon > player2RoundsWon ? 'Gast' : player2RoundsWon > player1RoundsWon ? player2Name : 'No one';

 
  function endGame() {
    gameOver = true;
  }

</script>

<div id="game-page">
  <!-- Round Counter Component -->
  <Rounds {round} {turnCount} {selectRandomStat} />

 <!-- Selected Stat Icon with bounce animation triggered on stat change -->
<div class="selected-stat-icon" key={selectedStat}>
  <img src={statIcons[selectedStat]} alt={selectedStat} class="bounce" />
</div>


  <!-- Player 1 -->
  <div class="player player1">
    <div class="circle profile-circle" style="background-image: url({player1Profile});"></div>
    <div class="player-name">
      <p>{player1Name}</p>
    </div>
  </div>

  <!-- Player 2 -->
  <div class="player player2">
    <div class="circle profile-circle" style="background-image: url({player2Profile});"></div>
    <div class="player-name">
      <p>{player2Name}</p>
    </div>
  </div>

  <!-- Player 1's Cards -->
  <div class="cards player-cards" player1-cards>
    {#each player1Cards as card (card.id)}
      <div class="card-wrapper" 
          data-card-data={JSON.stringify(card)} 
          on:click={() => isPlayer1Turn && updateSelectedCard(card)}>
        <Card animal={card} selectedStat={selectedStat} />
      </div>
    {/each}
  </div>

  <!-- Player 2's Cards -->
  <div class="cards player-cards" player2-cards>
    {#each player2Cards as card (card.id)}
      <div class="card-wrapper" 
          data-card-data={JSON.stringify(card)}>
        <img src="images/cardback.png" class="overlay-image" alt="Overlay Image">
        <Card animal={card} selectedStat={selectedStat} />
      </div>
    {/each}
  </div>

  <!-- Played Cards in the Middle -->
  <div class="played-cards-container {fadeOut ? (isTie ? 'fade-out-tie' : (player1Won ? 'fade-out-bottom-right' : 'fade-out-top-left')) : ''}">
    <div class="played-card played-card-left">
      {#if selectedCardName}
        <Card animal={data.find(c => c.name === selectedCardName)} selectedStat={selectedStat} />
      {/if}
    </div>
    <div class="played-card played-card-right">
      {#if computerSelectedCard}
        <Card animal={data.find(c => c.name === computerSelectedCard)} selectedStat={selectedStat} />
      {/if}
    </div>
  </div>


  {#if gameOver}
  <div class="game-over">
    <h3>
      {#if player1RoundsWon > player2RoundsWon}
        Glückwunsch!
      {:else if player2RoundsWon > player1RoundsWon}
        Oh nein!
      {:else}
        Unentschieden!
      {/if}
    </h3>
    <p>
      {#if player1RoundsWon > player2RoundsWon}
        Du hast das Duell gewonnen! 🎉
      {:else if player2RoundsWon > player1RoundsWon}
        {player2Name} hat das Duell gewonnen! 😢
      {:else}
        Das Duell endet ohne Sieger! 🤝
      {/if}
    </p>
    <button on:click={restartGame}>Nochmal</button>
    <a href="/">
      <button>Beenden</button>
    </a>
  </div>
{/if}

  <!-- Message Container -->
  <div class="message-container {message ? 'show' : ''}">
    {#if message}
      <p>{message}</p>
    {/if}
  </div>



  </div>

<style>
  .played-cards-container {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
    position: fixed;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 200px;
    height: 150px;
    transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out; /* Add transform transition */
  }
  .played-card {
    transform: scale(0.5);
    background: white;
    padding: 5px;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(255, 255, 255, 0.8), 0 0 10px rgba(0, 0, 0, 0.5);
    }
  .player1-card { margin-right: 10px; }
  .player2-card { margin-left: 10px; }
  .fade-out {
    opacity: 0;
  }
  
  .fade-out-bottom-right {
    opacity: 0;
    transform: translate(45vw, 45vh); /* Move to bottom right */
  }
  .fade-out-top-left {
    opacity: 0;
    transform: translate(-45vw, -45vh); /* Move to top left */
  }
  .fade-out-tie .played-card-left,
  .fade-out-tie .played-card-right {
    opacity: 0;
    transition: opacity 0.5s ease-in-out; /* Ensure smooth fade-out with reduced duration */
  }
  .bounce { 
    animation: bounce 1s ease-in-out;
  }
  @keyframes bounce {
    0%, 100% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.3);
    }
  }
  .message-container {
    position: absolute;
    right: 10%; /* Adjust to move more to the left */
    top: 40%;
    transform: translateY(-50%);
    width: 200px; /* Adjust width as needed */
    padding: 10px;
    background-color: #0b0b0d; 
    color: white;
    text-align: center;
    border-radius: 5px;
    opacity: 0;
    transition: opacity 0.5s ease-in-out;
  }

  .message-container.show {
    opacity: 1;
  }


</style>