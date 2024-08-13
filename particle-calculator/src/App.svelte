<!-- App.svelte -->
<script>
  import { writable } from 'svelte/store';
  import { fade, fly } from 'svelte/transition';

  let particleType = '';
  let result = writable({
    generalizedCoordinates: '',
    degreesOfFreedom: 0
  });
  let showResults = false;

  function calculateParticleProperties() {
    showResults = false;
    setTimeout(() => {
      switch (particleType.toLowerCase()) {
        case 'free particle':
          result.set({
            generalizedCoordinates: '(x, y, z)',
            degreesOfFreedom: 3
          });
          break;
        case 'particle on a line':
          result.set({
            generalizedCoordinates: '(x)',
            degreesOfFreedom: 1
          });
          break;
        case 'particle on a plane':
          result.set({
            generalizedCoordinates: '(x, y)',
            degreesOfFreedom: 2
          });
          break;
        case 'particle on a sphere':
          result.set({
            generalizedCoordinates: '(θ, φ)',
            degreesOfFreedom: 2
          });
          break;
        case 'rigid body':
          result.set({
            generalizedCoordinates: '(x, y, z, θ, φ, ψ)',
            degreesOfFreedom: 6
          });
          break;
        default:
          result.set({
            generalizedCoordinates: 'Unknown',
            degreesOfFreedom: 'Unknown'
          });
      }
      showResults = true;
    }, 300);
  }
</script>

<main>
  <div class="logo">PartiGen</div>
  <div class="search-container">
    <div class="search-bar">
      <input
        type="text"
        placeholder="Enter particle type..."
        bind:value={particleType}
        on:input={calculateParticleProperties}
      />
      <button on:click={calculateParticleProperties}>
        <svg viewBox="0 0 24 24">
          <path fill="currentColor" d="M15.5 14h-.79l-.28-.27C15.41 12.59 16 11.11 16 9.5 16 5.91 13.09 3 9.5 3S3 5.91 3 9.5 5.91 16 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"/>
        </svg>
      </button>
    </div>
  </div>

  {#if showResults}
    <div class="results-card" in:fly="{{ y: 50, duration: 300 }}" out:fade>
      <h2>Results:</h2>
      <p>Generalized Coordinates: <strong>{$result.generalizedCoordinates}</strong></p>
      <p>Degrees of Freedom: <strong>{$result.degreesOfFreedom}</strong></p>
    </div>
  {/if}
</main>

<style>
  :global(body) {
    font-family: 'Roboto', Arial, sans-serif;
    background-color: #f1f3f4;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  main {
    width: 100%;
    max-width: 600px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .logo {
    font-size: 64px;
    font-weight: 500;
    margin-bottom: 20px;
    background: linear-gradient(45deg, #4285f4, #34a853, #fbbc05, #ea4335);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    text-fill-color: transparent;
  }

  .search-container {
    display: flex;
    justify-content: center;
    margin-bottom: 30px;
    width: 100%;
  }

  .search-bar {
    display: flex;
    align-items: center;
    background-color: white;
    border-radius: 24px;
    padding: 6px 20px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    width: 100%;
    max-width: 500px;
    transition: box-shadow 0.3s;
  }

  .search-bar:hover, .search-bar:focus-within {
    box-shadow: 0 3px 8px rgba(0,0,0,0.2);
  }

  input {
    flex-grow: 1;
    border: none;
    font-size: 16px;
    padding: 10px 0;
    outline: none;
  }

  button {
    background: none;
    border: none;
    cursor: pointer;
    padding: 10px;
    color: #4285f4;
  }

  button svg {
    width: 24px;
    height: 24px;
  }

  .results-card {
    background-color: white;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    width: 100%;
    max-width: 500px;
  }

  h2 {
    color: #202124;
    font-size: 18px;
    margin-top: 0;
  }

  p {
    color: #5f6368;
    font-size: 14px;
  }

  strong {
    color: #202124;
  }
</style>
