<script>
  export let animal;
  export let selectedStat; // New prop to indicate the selected stat for the current round

  // Function to get the background color based on groupname
  function getBackgroundColor(groupname) {
    switch (groupname.toLowerCase()) {
      case 'predators':
        return '#bf3328';
      case 'poisonous and infectious':
        return '#bd712d';
      case 'reptiles':
        return '#2a632a';
      case 'sea creatures':
        return '#61499e';
      case 'marine giants':
        return '#187cab';
      case 'large mammals':
        return '#8c6824';
      case 'land mammals':
        return '#859632';
      case 'birds':
        return '#2da69a';
      default:
        return 'black';
    }
  }
</script>

<div class="card-wrapper" style="background-color: {getBackgroundColor(animal.groupname)};">
  <div class="card-content">
    <div class="card-header">
      <!-- Group Number -->
      <div class="card-number group-{animal.group.toLowerCase()}">
        {animal.group.toUpperCase()}{animal.group_number}
      </div>
      <!-- Card Title -->
      <div class="card-title">{animal.name_german}</div>
    </div>

    <!-- Group Name Icon -->
    <div class="groupname-icon" style="background-color: {getBackgroundColor(animal.groupname)};">
      <img src={`icons/${animal.groupname.toLowerCase().replace(/\s+/g, '_')}.png`} alt="{animal.groupname_german}" />
    </div>

    <!-- Card Image -->
    <img class="card-image" src={`images/${animal.group.toLowerCase()}${animal.group_number}.jpeg`} alt="{animal.name_german}" />

    <!-- Trivia -->
    <div class="card-trivia">{animal.trivia_german}</div>

    <!-- Statistics -->
    <div class="card-statistics">
      <div class="statistics-pair {selectedStat === 'max_weight' ? 'selected-stat' : ''}">
        <div class="stat-name">
          <img src="icons/weight.png" alt="weight">
        </div>
        <div class="stat-value">{animal.max_weight}</div>
      </div>
      <div class="statistics-pair {selectedStat === 'max_length' ? 'selected-stat' : ''}">
        <div class="stat-name">
          <img src="icons/length.png" alt="length">
        </div>
        <div class="stat-value">{animal.max_length}</div>
      </div>
      <div class="statistics-pair {selectedStat === 'max_age' ? 'selected-stat' : ''}">
        <div class="stat-name">
          <img src="icons/age.png" alt="maximum age">
        </div>
        <div class="stat-value">{animal.max_age}</div>
      </div>
      <div class="statistics-pair {selectedStat === 'top_speed' ? 'selected-stat' : ''}">
        <div class="stat-name">
          <img src="icons/speed.png" alt="maximum speed">
        </div>
        <div class="stat-value">{animal.top_speed}</div>
      </div>
      <div class="statistics-pair {selectedStat === 'litter_size' ? 'selected-stat' : ''}">
        <div class="stat-name">
          <img src="icons/offspring.png" alt="offspring count per cycle">
        </div>
        <div class="stat-value">{animal.litter_size}</div>
      </div>
      <div class="statistics-pair {selectedStat === 'deaths' ? 'selected-stat' : ''}">
        <div class="stat-name">
          <img src="icons/death.png" alt="caused human casualties per year">
        </div>
        <div class="stat-value">{animal.deaths}</div>
      </div>
    </div>
  </div>
</div>

<style>
  .card-wrapper {
    padding: 12px;
    display: inline-block;
    transition: opacity 0.3s ease; /* Smooth opacity transition */
  }

  .card-content {
    background: linear-gradient(rgb(20, 63, 84), rgb(9, 12, 22)); /* Gradient */
    width: 250px;
    height: 400px;
    padding: 1rem;
    display: grid;
    grid-template-rows: repeat(10, 1fr);
    border-radius: 10px;
    position: relative;
    overflow: hidden;
  }

  .card-number {
    font-weight: 600;
    top: 0.8rem;
    color: rgb(0, 0, 0);
    display: flex;
    align-items: center;
    justify-content: center;
    width: 15%;
    height: 8%;
    font-size: 0.8rem;
    background-color: rgb(255, 255, 255);
    border-radius: 0.5rem;
    position: absolute;
  }

  .card-title {
    font-weight: 600;
    position: relative;
    top: 0.3rem;
    color: white;
    font-size: 1rem;
    text-align: center;
  }

  .card-image {
    object-fit: cover;
    width: 100%;
    height: 85%;
    max-width: 100%;
    max-height: 100%;
    grid-row: span 5;
  }

  .card-trivia {
    background-color: rgb(42, 47, 70);
    color: white;
    position: relative;
    font-size: .7rem;
    padding: 0.5rem;
    text-align: center;
    height: 2.8rem;
    display: flex;
    align-items: center;
    border-radius: 0.5rem;
    margin-top: -20px;
  }

  .card-statistics {
    grid-row: span 3;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: repeat(3, 1fr);
    gap: 0.5rem;
    color: white;
  }

  .statistics-pair {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background-color: rgb(42, 47, 70);
    border-radius: 0.5rem;
    padding: 0.2rem;
  }

  /*.statistics-pair.selected-stat {
    background-color: rgba(122, 195, 255, 0.366); 
  } 
*/
  .stat-name, .stat-value {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.9rem;
    color: white;
  }

  .stat-name img {
    max-width: 65%;
    object-fit: contain;
  }

  .stat-value {
    flex: 1;
    text-align: right;
  }

  .groupname-icon {
    position: absolute;
    right: 14px;
    top: 0.8rem;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 15%;
    height: 8%;
    border-radius: 0.5rem;
  }

  .groupname-icon img {
    height: 25px;
    width: 25px;
  }
</style>