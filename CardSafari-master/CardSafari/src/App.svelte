<script>
  import { Link, Route, Router, navigate } from 'svelte-routing';
  import HomePage from './pages/HomePage.svelte';
  import CollectionPage from './pages/CollectionPage.svelte';
  import ConnectingPage from './pages/ConnectingPage.svelte';
  import GamePage from './pages/GamePage.svelte';

  function scrollToRules(event) {
    event.preventDefault();
    sessionStorage.setItem('scrollToRules', 'true');
    navigate('/#rules', { replace: true });
  }

  function scrollToTop(event) {
    event.preventDefault();
    sessionStorage.removeItem('scrollToRules');
    navigate('/');
    setTimeout(() => {
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }, 100);
  }
</script>

<Router>
  <div id="app-container">
    <header>
      <!-- Left: Logo and Title -->
      <div class="logo-container">
        <Link to="/">
          <img src="icons/logo.png" alt="Card Safari Logo" />
        </Link>
        <Link to="/" class="title-link">
          <h2>Card Safari</h2>
        </Link>
      </div>

      <!-- Right: Navigation -->
      <nav>
        <ul>
          <li><Link to="/collection" class="active">Sammlung</Link></li>
          <li><Link to="/connecting">Spielen</Link></li>
          <li><a href="#" on:click={scrollToRules}>Regeln</a></li>
          <li class="profile-container">
            <img src="images/Profile/profile.jpg" alt="Profile" class="profile-image">
          </li>
        </ul>
      </nav>
    </header>

    <main>
      <Route path="/" component={HomePage} />
      <Route path="/collection" component={CollectionPage} />
      <Route path="/connecting" component={ConnectingPage} />
      <Route path="/game" component={GamePage} />
    </main>

    <footer>
      <p>© 2025 Card Safari – Alle Rechte vorbehalten.</p>
    </footer>
  </div>
</Router>

<style>
  #app-container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }

  main {
    flex: 1;
  }

  .controls {
    display: flex;
    justify-content: space-between;
    padding: 1rem;
  }

  .title-link {
    text-decoration: none;
    color: inherit;
  }

  .title-link h1 {
    cursor: pointer;
    margin: 0;
  }

  .profile-container {
    margin-left: auto;
  }

  .profile-image {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    cursor: pointer;
  }
</style>
