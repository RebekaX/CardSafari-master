<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  let selectedGroupname = 'Alle';

  function handleFilterChange(event) {
    selectedGroupname = event.target.value;
    dispatch('filterChange', { groupname: selectedGroupname });
    closeDropdown();
  }

  function closeDropdown() {
    document.getElementById('dropdown').checked = false;
  }
</script>

<div class="sec-center">
  <input class="dropdown" type="checkbox" id="dropdown" />
  <label class="for-dropdown" for="dropdown">
    {#if selectedGroupname === 'Alle'}
    Filter <span style="margin-left: 5px;">&#x25BE;</span>
{:else}
    {selectedGroupname} <span style="margin-left: 5px;">&#x25BE;</span>
{/if}
<i class="uil uil-arrow-down"></i>
</label>
  <div class="section-dropdown">  
    <ul>
      <li><a href="#" on:click={() => handleFilterChange({ target: { value: 'Alle' } })}>Alle</a></li>
      <li><a href="#" on:click={() => handleFilterChange({ target: { value: 'Raubtiere' } })}>Raubtiere</a></li>
      <li><a href="#" on:click={() => handleFilterChange({ target: { value: 'Giftig und infektiös' } })}>Giftig und infektiös</a></li>
      <li><a href="#" on:click={() => handleFilterChange({ target: { value: 'Reptilien' } })}>Reptilien</a></li>
      <li><a href="#" on:click={() => handleFilterChange({ target: { value: 'Meeresbewohner' } })}>Meeresbewohner</a></li>
      <li><a href="#" on:click={() => handleFilterChange({ target: { value: 'Meeresgiganten' } })}>Meeresgiganten</a></li>
      <li><a href="#" on:click={() => handleFilterChange({ target: { value: 'Großsäuger' } })}>Großsäuger</a></li>
      <li><a href="#" on:click={() => handleFilterChange({ target: { value: 'Landsäugetiere' } })}>Landsäugetiere</a></li>
      <li><a href="#" on:click={() => handleFilterChange({ target: { value: 'Vögel' } })}>Vögel</a></li>
    </ul>
  </div>
</div>

<style>
  * {
    box-sizing: border-box;
  }

  body {
    font-weight: 500;
    font-size: 15px;
    line-height: 1.4;
    color: #fff;
    background-color: #0b0b0d;
    overflow-x: hidden;
    background-image: url('https://s3-us-west-2.amazonaws.com/s.cdpn.io/1462889/pat-back.svg');
    background-position: center;
    background-repeat: repeat;
    background-size: 4%;
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    padding-top: 100px;
    padding-bottom: 300px;
  }

  .sec-center {
    position: relative;
    max-width: 100%;
    text-align: center;
    z-index: 200;
  }

  [type="checkbox"]:checked,
  [type="checkbox"]:not(:checked) {
    position: absolute;
    left: -9999px;
    opacity: 0;
    pointer-events: none;
  }

  .dropdown:checked + label,
  .dropdown:not(:checked) + label {
    position: relative;
    font-weight: 600;
    font-size: 15px;
    line-height: 2;
    height: 50px;
    transition: all 200ms linear;
    border-radius: 4px;
    width: 220px;
    letter-spacing: 1px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    border: none;
    background-color: #e1e1e1;
    cursor: pointer;
    color: #0d0e11;
    box-shadow: 0 14px 35px 0 rgba(4, 4, 7, 0.6);
  }

  .dropdown:checked + label:before,
  .dropdown:not(:checked) + label:before {
    position: fixed;
    top: 0;
    left: 0;
    content: '';
    width: 100%;
    height: 100%;
    z-index: -1;
    cursor: auto;
    pointer-events: none;
  }

  .dropdown:checked + label:before {
    pointer-events: auto;
  }

  .dropdown:not(:checked) + label .uil {
    font-size: 24px;
    margin-left: 10px;
    transition: transform 200ms linear;
  }

  .dropdown:checked + label .uil {
    transform: rotate(180deg);
    font-size: 24px;
    margin-left: 10px;
    transition: transform 200ms linear;
  }

  .section-dropdown {
    position: absolute;
    padding: 5px;
    background-color: #0b0b0d;
    top: 70px;
    left: 0;
    width: 100%;
    border-radius: 4px;
    display: block;
    box-shadow: 0 14px 35px 0 rgba(9, 9, 12, 0.4);
    z-index: 2;
    opacity: 0;
    pointer-events: none;
    transform: translateY(-20px);
    transition: all 200ms linear;
  }

  .dropdown:checked ~ .section-dropdown {
    opacity: 1;
    pointer-events: auto;
    transform: translateY(0);
  }

  .section-dropdown:before {
    position: absolute;
    top: -20px;
    left: 0;
    width: 100%;
    height: 20px;
    content: '';
    display: block;
    z-index: 1;
  }

  .section-dropdown:after {
    position: absolute;
    top: -7px;
    left: 30px;
    width: 0;
    height: 0;
    border-left: 8px solid transparent;
    border-right: 8px solid transparent;
    border-bottom: 8px solid #0b0b0d;
    content: '';
    display: block;
    z-index: 2;
    transition: all 200ms linear;
  }

  .section-dropdown ul {
    list-style-type: disc;
    padding-left: 20px;
  }

  .section-dropdown ul li::marker {
    color: #9340b7;
  }

  a {
    position: relative;
    color: #fff;
    transition: all 200ms linear;
    font-weight: 500;
    font-size: 15px;
    border-radius: 2px;
    padding: 5px 0;
    padding-left: 20px;
    padding-right: 15px;
    margin: 2px 0;
    text-align: left;
    text-decoration: none;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  a:hover {
    color: #102770;
    background-color: #9340b7;
  }

  a .uil {
    font-size: 22px;
  }
</style>