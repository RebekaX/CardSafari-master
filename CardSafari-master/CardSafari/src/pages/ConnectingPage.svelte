<script>
    import { onMount } from 'svelte';
    import { Link, navigate } from 'svelte-routing';

    let isReady = false;
    let playerName = "Gast"; // You can replace this with actual player data if needed
    let player2Name = "Lädt..."; // Default text until API response is received
    let connectingText = "Verbindet..."; // Initial connecting text
    let showButtons = true;
    let screenBlack = false;
    let player2ProfileImage = '';
    const profileImages = ['images/Profile/profile2.jpg','images/Profile/profile3.jpg', 'images/Profile/profile4.jpg', 'images/Profile/profile5.jpg'];
    player2ProfileImage = profileImages[Math.floor(Math.random() * profileImages.length)];
    // Function to fetch a random username for Player 2 using Randommer.io API
    async function fetchRandomUsername() {
        const apiKey = '4af85c68f95a4c0baca506a8ec03382e'; // Replace with your actual API key from Randommer.io
        const url = 'https://randommer.io/api/Name?nameType=firstname&quantity=1'; // API URL for username generation

        try {
            const response = await fetch(url, {
                method: 'GET',
                headers: {
                    'x-api-key': apiKey,  // Include the API key in the header
                },
            });

            if (!response.ok) {
                throw new Error("Failed to fetch username");
            }

            const data = await response.json();
            console.log(data); // Log the response to inspect its structure

            // Assuming the response returns a structure like { data: ['name'] }
            if (data.length > 0) {
                player2Name = data[0]; // Using the first name from the response
            } else {
                player2Name = "Player 2"; // Default if no data
            }
        } catch (error) {
            console.error("Error fetching username:", error);
            player2Name = "Player 2"; // Default to "Player 2" if the API fails
        }
    }

    // Fetch the random username when the component is mounted
    onMount(() => {
        fetchRandomUsername();
        setTimeout(() => {
            connectingText = "Bereit!";
        }, 2000);
    });

    function toggleReady() {
        isReady = !isReady;
        if (isReady) {
            showButtons = false;
            connectingText = "Bereit!";
            setTimeout(() => {
                screenBlack = true;
                // Save player names and profile image to localStorage
                localStorage.setItem('player1Name', playerName);
                localStorage.setItem('player2Name', player2Name);
                localStorage.setItem('player2ProfileImage', player2ProfileImage);
                setTimeout(() => {
                    navigate(`/game?player1=${playerName}&player2=${player2Name}`);
                }, 1000); // Adjust the delay as needed
            }, 2000);
        }
    }

    onMount(() => {
        window.scrollTo(0, 0); // Scrolls the window to the top immediately
        document.body.style.overflow = 'hidden';

    return () => {
        // Remove the class when leaving the page
        document.body.style.overflow = '';
    };

  
});

</script>

<div id="connecting" class:screen-black={screenBlack}>
    <div class="main-container">
        <!-- Left side for Player 1 -->
        <div class="left-side">
            <div class="circle-container">
                <div class="circle profile-circle" style="background-image: url('images/Profile/profile.jpg');"></div>
                <div class="player-name">
                    <p>{playerName}</p> <!-- Player 1's name -->
                </div>
                {#if showButtons}
                <div class="buttons">
                    <Link to="/">
                        <button>Zurück</button>
                    </Link>
                    <button on:click={toggleReady}>{isReady ? 'Cancel' : 'Bereit!'}</button>
                </div>
                {/if}
                {#if !showButtons}
                <div class="ready-text">
                    <p>Bereit!</p>
                </div>
                {/if}
            </div>
        </div>

        <!-- Right side for Player 2 -->
        <div class="right-side">
            <div class="circle player2-circle" style="background-image: url({player2ProfileImage});">
                <!-- Player 2's profile picture -->
            </div>
            <div class="player-name">
                <p>{player2Name}</p> <!-- Player 2's name -->
            </div>
            <div class="connecting-text">
                <p>{connectingText}</p>
            </div>
        </div>
    </div>
</div>

<style>
    /* Preventing scrolling only on this page */
    html, body {
        margin: 0;
        padding: 0;
        height: 100%;
        overflow: hidden; /* Prevent scrolling */
    }

    #connecting {
        height: 100vh; /* Take up the whole viewport height */
        width: 100%; /* Take up the whole viewport width */
        display: flex;
        flex-direction: row; /* Ensure left and right sides are side by side */
        background: linear-gradient(-60deg, rgba(168, 45, 45, 0.5) 45%, rgba(85, 85, 221, 0.5) 55%); /* 60-degree diagonal gradient with reduced opacity */
        transition: background 3s; /* Smooth transition for background color */
    }
    #connecting::before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: #0b0b0d;
        z-index: -1; /* Place it behind the gradient background */
    }

    .screen-black {
        background: black; /* Change background to black */
    }

    .main-container {
        display: flex;
        width: 100%;
        height: 100%; /* Ensure the container fills the full height */
    }

    .left-side, .right-side {
        flex: 1; /* Ensure each side takes up half of the screen */
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column; /* Stack elements vertically in left and right sections */
    }

    .circle-container {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column; /* Stack profile circle and name vertically */
    }

    .circle {
        width: 150px;
        height: 150px;
        border-radius: 50%;
        border: 2px solid #ffffff;
        display: flex;
        flex-direction: column; 
        justify-content: center;
        align-items: center;
        background-size: cover;
        background-position: center;
    }

    .profile-circle {
        background-image: url('images/Profile/profile.jpg'); /* Player 1's profile image */
    }

    .player2-circle {
        background-size: cover;
        background-position: center;
    }

    .player-name {
        margin-top: 10px;
        color: white; /* Ensuring name is visible on blue background */
        font-size: 1.2rem;
    }

    .connecting-text {
        margin-top: 10px;
        color: white; /* Ensuring text is visible on blue background */
        font-size: 1.2rem;
    }

    .ready-text {
        margin-top: 10px;
        color: white; /* Ensuring text is visible on blue background */
        font-size: 1.2rem;
    }

    .buttons {
        display: flex;
        justify-content: center; /* Center buttons horizontally */
        gap: 10px; /* Space between the buttons */
        margin-top: 3rem;
    }

    button {
        padding: 0.5rem 1rem;
        font-size: 1rem;
        cursor: pointer;
        width: 8rem;
    }
</style>