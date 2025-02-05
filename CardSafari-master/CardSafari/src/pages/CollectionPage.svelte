<script>
    import { onMount } from 'svelte';
    import data from '../assets/animaldata.js';
    import Card from '../components/Card.svelte';
    import Filter from '../components/Filter.svelte';

    let filteredData = data;
    let selectedCategory = '';
    let randomAnimals = []; // Store the 10 randomly selected animals
    let collectedCards = []; // Array to track collected cards

    const categories = ['Mammals', 'Birds', 'Reptiles', 'Fish']; // Example categories

    // Filter function
    function filterData(category) {
        selectedCategory = category;
        filteredData = category ? data.filter((animal) => animal.category === category) : data;
    }

    // Handle filter change
    function handleFilterChange(event) {
        const { groupname } = event.detail;
        if (groupname === 'Alle') {
            filteredData = data;
        } else {
            filteredData = data.filter(animal => animal.groupname_german === groupname);
        }
        // Sort the filtered data by group_number
        filteredData.sort((a, b) => a.group_number - b.group_number);
    }

    // Randomly select 10 animals
    function selectRandomAnimals() {
        const randomIndexes = new Set();
        while (randomIndexes.size < 15) {
            randomIndexes.add(Math.floor(Math.random() * filteredData.length));
        }
        randomAnimals = Array.from(randomIndexes).map(index => filteredData[index]);
    }

    // Handle card collection
    function handleCardCollection(animal) {
        if (!collectedCards.includes(animal.id)) {
            collectedCards = [...collectedCards, animal.id];
            localStorage.setItem('collectedCards', JSON.stringify(collectedCards)); // Store collected cards in local storage
        }
    }

 // On mount, apply initial filter and select random animals
onMount(() => {
    // Scroll to the top of the page when the component is mounted
    window.scrollTo(0, 0);
    
    selectRandomAnimals();
    console.log('Collected cards:', randomAnimals);
});
</script>

<div id="wrapper">
    <!-- Filters -->
    <div class="controls">
        <Filter {categories} {selectedCategory} onCategoryChange={filterData} on:filterChange={handleFilterChange} />
    </div>

    <!-- Animal Cards -->
    <main>
        {#each filteredData as animal}
            <div class="card">
                <Card {animal} 
                      isRandom={randomAnimals.includes(animal)} 
                      isCollected={collectedCards.includes(animal.id)} 
                      onCardCollect={handleCardCollection} />
            </div>
        {/each}
    </main>
</div>

<style>
    .controls {
        display: flex;
        justify-content: space-between;
        padding: 1rem;
    }

    .card {
        transform: scale(0.9); /* Scale the cards to 0.9 */
    }
</style>