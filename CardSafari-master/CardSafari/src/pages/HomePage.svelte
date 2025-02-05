<script>
  import { Link } from 'svelte-routing';
  import { onMount } from 'svelte';
  import data from '../assets/animaldata.js';
  import Card from '../components/Card.svelte';
  import '../app.css';
  import '../mobile.css'; // Import mobile.css after other stylesheets

  let randomCards = [];
  let funFact = ''; // To store the animal fact
  let isLoading = false;
  let loadingMessage = '';

  const loadingMessages = [
    "Unsere tierischen Freunde sind noch am Forschen… fast fertig! 🐒",
    "Geduld, gleich wird’s tierisch spannend! 🎴🦁",
    "Die Tiere tippen noch – es wird wild! 💻🦥 "
  ];

  onMount(() => {
    randomCards = getRandomCards(2);
    fetchFunFact();

    // Check if the session storage contains the flag for scrolling to rules
    const scrollToRules = sessionStorage.getItem('scrollToRules');
    if (scrollToRules === 'true') {
      setTimeout(() => {
        document.getElementById('rules').scrollIntoView({ behavior: 'smooth' });
        sessionStorage.removeItem('scrollToRules'); // Reset the flag
      }, 100);
    } else {
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }
  });

  function getRandomCards(count) {
    const shuffledData = data.sort(() => 0.5 - Math.random());
    return shuffledData.slice(0, count);
  }

  async function fetchFunFact() {
    isLoading = true;
    loadingMessage = loadingMessages[Math.floor(Math.random() * loadingMessages.length)];
    try {
      const selectedCard = randomCards[Math.floor(Math.random() * randomCards.length)];
      const response = await fetch('https://api.openai.com/v1/chat/completions', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer sk-pro`,
        },
        body: JSON.stringify({
          model: 'gpt-4',
          messages: [
            {
              role: 'system',
              content: 'Du bist ein hilfreicher Assistent, der lustige, kurze und interessante Fakten über Tiere generiert meistens mit ein bisschen humor, generiere nur die Fakten die 2 Sätze lang sind und duze schreeib nur den fakt also ohne "Wusstest du schon" ',
            },
            {
              role: 'user',
              content: `Erstelle eine kurze und interessante Tatsache über das folgende Tier: ${selectedCard?.name} versuche dabei entweder eine der Kategorien wie größe, tötlichkeit, schnelligkeit, gewicht, alter, wurf zu verwenden oder etwas ganz anderes was interessant oder lustig ist `,
            },
          ],
          temperature: 0.7,
        }),
      });

      if (!response.ok) {
        throw new Error('Fehler beim Abrufen der Tierfakten');
      }

      const data = await response.json();
      funFact = data.choices[0].message.content.trim();
    } catch (error) {
      console.error('Fehler beim Abrufen der Tierfakten:', error);
    } finally {
      isLoading = false;
    }
  }
</script>

<div id="home">
  <div class="image-container">
    <img src="images/thumbnail01.png" alt="Card Safari" class="full-width-image">
    <h1 class="centered-title">
      Willkommen bei<br />
      <span class="highlighted-text">Card Safari!</span>
    </h1>
  </div>

  <main>
    <div class="button-container">
      <div class="background-rectangle"></div>
      <Link to="/collection">
        <button>Sammlung</button>
      </Link>
      <Link to="/connecting">
        <button>Spielen</button>
      </Link>
    </div>

    <div class="content-container">
      <div class="text-content">
        <h3 class="common-heading">Erweitere deine Sammlung!</h3>
        <p class="info-text">
          Sammle alle 32 Karten, setze dein Wissen ein und werde zum Profi im Tierkartenspiel, während du spielerisch alles
           über die faszinierende Welt der Tiere entdeckst – von winzigen Insekten bis hin zu gigantischen Meeresbewohnern!
        </p>

        <div class="fun-fact">
          <h3 class="common-heading">Wusstest du schon?</h3>
          <div class="fun-fact-container">
            {#if isLoading}
              <div class="loading-container">
                <div class="loading-spinner"></div>
                <p>{loadingMessage}</p>
              </div>
            {:else}
              <p>{funFact}</p>
            {/if}
          </div>
          <button on:click={fetchFunFact}>Neuer Fakt</button>
        </div>

        <div class="how-it-works" id="rules">
          <h3 class="common-heading">So funktioniert es</h3>
          <p class="info-text">Entwickle deine eigene Strategie und werde zum Meister der Tierkarten! Beobachte deine Gegner, wähle geschickt und sichere dir den Sieg.</p>
          <div class="hexagon-container">
            <div class="hexagon-item">
              <img src="icons/hex1.png" alt="Hexagon" class="hexagon">
              <p class="bold-text">1. Karten im Duell</p>
              <p>Jeder Spieler erhält 5 Karten mit einzigartigen Tieren und deren Werten.</p>
            </div>
            <div class="hexagon-item">
              <img src="icons/hex2.png" alt="Hexagon" class="hexagon">
              <p class="bold-text">2. Spannende Runden </p>
              <p>In jeder Runde wird zufällig einer der sechs Werte gewählt – der höchste gewinnt!</p>
            </div>
            <div class="hexagon-item">
              <img src="icons/hex3.png" alt="Hexagon" class="hexagon">
              <p class="bold-text">3. Gewinne das Duell</p>
              <p>Jede gewonnene Runde bringt dich dem Sieg näher. Der Spieler mit den meisten Siegen gewinnt das Duell!</p>
            </div>
          </div>
        </div>
      </div>

      <div class="random-cards">
        {#each randomCards as card, index (card.id)}
          <div class="card-wrapper" style="transform: rotate({index === 0 ? '-10deg' : '20deg'}); z-index: {index === 0 ? 2 : 1}; margin-top: {index === 0 ? '-1rem' : '-9rem'}; margin-right: {index === 0 ? '10rem' : '0'};">
            <Card animal={card} />
          </div>
        {/each}
      </div>
    </div>
  </main>
</div>

<style>

  /* Existing styles */
  #home {
    text-align: left;
    width: 100%;
  }

  .image-container {
    position: relative;
    display: inline-block;
    width: 100%;
  }

  .full-width-image {
    width: 100%;
    height: 40rem;
    object-fit: cover;
  }

  .centered-title {
    position: absolute;
    top: 40%;
    right: 10%; 
    transform: translate(0, -50%);
    color: white;
    font-size: 3.5rem;
    font-weight: bold;
    text-align: left;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
  }

  .highlighted-text {
    font-size: 5rem;
  }

  .content-container {
    display: flex;
    justify-content: space-between;
    padding: 2rem;
    padding-left: 2.5rem;
    padding-top: 0.5rem;
    padding-bottom: 30rem;
  }

  .text-content {
    flex: 1;
    max-width: 48%;
  }

  .info-text {
    text-align: left;
    color: #ffffff;
    font-size: 1.1rem;
    line-height: 1.4;
    margin: 2rem;
  }

  .fun-fact {
    margin-top: 5rem;
    padding: 1rem;
    border: 2px solid #9340b7;
    background-color: transparent;
    border-radius: 12px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    margin-left: 1rem;
    margin-bottom: 8rem;
    margin-top: 8rem;
  }

  .fun-fact h3 {
    color: #ffffff;
    font-size: 1.5rem;
    margin-left: 0;  
    margin-bottom: 0.5rem;
    margin-top: 0;
  }
  .common-heading {
    font-size: 2rem;
    font-weight: bold;
    color: #ffffff;
    margin-bottom: 0.5rem;
    margin-left: 2rem;
    margin-top: 0;
  }

  .fun-fact p {
    color: #ffffff;
    font-size: 1rem;
    margin: 0;
    margin-top: 0.5rem;
    line-height: 1.4;
  }

  .fun-fact button {
    align-self: flex-end;
    margin: 0.5rem;
    padding: 0.5rem 1rem;
    margin-top: 0.8rem;
    color: white;
    border-radius: 8px;
    cursor: pointer;
    font-size: 0.9rem;
    transform: scale(0.9);
  }

  .fun-fact button:hover {
  }

  .random-cards {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    transform: scale(0.7);
    margin-top: -6rem;
    margin-right: 2rem;
  }

  .card-wrapper {
    margin-bottom: 1rem;
  }

  main {
    display: flex;
    flex-direction: column;
    align-items: left;
    position: relative;
  }

  .loading-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .loading-spinner {
    border: 4px solid #0b0b0d;
    border-left-color: #9340b7;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    animation: spin 1s linear infinite;
  }

  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }

  .button-container {
    position: relative;
    text-align: center;
    margin-bottom: 2rem;
  }

  .background-rectangle {
    position: absolute;
    top: 50%;
    left: 0;
    width: 100%;
    height: 20rem;
    background: linear-gradient(to bottom, #0b0b0d, rgba(27,27,45, 0.5));
    z-index: -1;
    transform: translateY(-50%);
  }

  .how-it-works {
    margin-top: 3rem;
    position: relative;
    height: 10rem; /* Adjust height as needed */
  }

  .hexagon-container {
    position: absolute;
    top: 90%;
    left: 100%;
    transform: translate(-50%, -50%);
    display: flex;
    gap: 10rem;
    margin-top: 13rem;
  }

  .hexagon {
    width: 15em;
    height: 15rem;
  }

  .hexagon-item {
    text-align: center;
  }

  .hexagon-item p {
    color: #ffffff;
  }

  .bold-text {
    font-weight: bold;
  }
</style>