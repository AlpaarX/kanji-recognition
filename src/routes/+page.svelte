<script>
  // @ts-nocheck

  import data from "../components/kanji.json";

  let kanjiList = Object.entries(data);
  let knowCounter = 0;
  let dknowCounter = 0;
  let counter = 0;
  let remaining = kanjiList.length - (knowCounter + dknowCounter);

  function calcRemaining() {
    remaining = kanjiList.length - (knowCounter + dknowCounter);
  }
  function know() {
    calcRemaining();
    knowCounter += 1;
    if (knowCounter > 0 && kanjiList[counter].know == false) {
      dknowCounter -= 1;
      kanjiList[counter].know = true;
      next();
    } else {
      kanjiList[counter].know = true;
      next();
    }
  }
  function dknow() {
    dknowCounter += 1;
    calcRemaining();
    if (knowCounter > 0 && kanjiList[counter].know == true) {
      knowCounter -= 1;
      kanjiList[counter].know = false;
      next();
    } else {
      next();
    }
  }
  function next() {
    if (counter < kanjiList.length - 1) {
      counter += 1;
    }
  }
  function back() {
    if (counter > 0) {
      counter -= 1;
    }
  }

  // Favorites
  /**
   * @type {any[]}
   */
  let favorites = [];

  function addFav() {
    favorites = [...favorites, kanjiList[counter][0]];
  }
</script>

<div class="wrap">
  <h1 class="title">Kanji Recognition Test</h1>
  <div class="top">
    <div class="fav">
      <button class="outline favorites" on:click={addFav}>❤️</button>
    </div>
    {#if kanjiList[counter].know == true}
      <p class="mark">⭕️</p>
    {:else}
      <p class="mark">❌</p>
    {/if}
    <h2>
      Meaning:
      {#each kanjiList[counter][1].wk_meanings as meaning}
        <span>{meaning.replace("^", ", ")}</span>
      {/each}
    </h2>
    <h3>Frequency: {kanjiList[counter][1].freq}</h3>
    {#each kanjiList[counter][1].wk_readings_on as on}
      <span>{on.replace("!", ", ")}</span>
    {/each}
    {#each kanjiList[counter][1].wk_readings_kun as kun}
      <span>{kun.replace("!", ", ")}</span>
    {/each}
    <h1 class="display-kanji">
      {kanjiList[counter][0]}
    </h1>
  </div>
  <div class="btns">
    <button id="" on:click={back}>←</button>
    <button id="dknow" on:click={dknow}>Don't know</button>
    <button id="know" on:click={know}>Know</button>
    <button id="" on:click={next}>→</button>
  </div>
  <h2>Know: {knowCounter}</h2>
  <h2>Don't know: {dknowCounter}</h2>
  <h2>
    Remaining: {remaining} / {kanjiList.length}
  </h2>
  <div class="saved">
    <h1 class="title">Saved</h1>

    <div class="fav-list">
      {#each favorites as fav}
        <h1>{fav}</h1>
      {/each}
    </div>
  </div>
  <!-- <div class="kanji-list">
    {#each kanjiList as kanji}
      <span class="display-kanji">
        {kanji[counter][0]}
      </span>
    {/each}
  </div> -->
</div>

<style>
  h1,
  h2,
  h3,
  h4,
  h5 {
    padding: 0;
    margin: 0;
  }
  .wrap {
    margin: 0 1rem;
  }
  .title {
    font-weight: 200;
  }
  .top {
    text-align: center;
    margin: 10rem 0;
  }
  .mark {
    margin: 0;
  }
  .display-kanji {
    font-size: 8rem;
  }
  .btns {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 0.5rem;
  }
  .fav {
    display: flex;
    justify-content: end;
  }
  .favorites {
    width: min-content;
  }
  .saved {
    margin-top: 2rem;
    text-align: center;
    background-color: #252429;
  }
  .fav-list {
    display: grid;
    grid-template-columns: repeat(16, 1fr);
  }
</style>
