<script>
  let buttons = new Array(9).fill(null);
  let result = null;
  let turn = 'X';
  let playerXWins = 0;
  let playerOWins = 0;
  let previousStates = [];

  const winCombinations = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  function setValue(i) {
    // Store the previous state before updating
    previousStates.push([...buttons]);

    buttons[i] = turn;
    buttons = [...buttons];
    turn = turn == 'X' ? 'O' : 'X';

    if (!buttons.includes(null)) {
      result = 'Match draw';
    } else {
      calculateWinner();
    }
  }

  function calculateWinner() {
    for (let i = 0; i < winCombinations.length; i++) {
      if (buttons[winCombinations[i][0]] !== null) {
        if (
          buttons[winCombinations[i][0]] == buttons[winCombinations[i][1]] &&
          buttons[winCombinations[i][1]] == buttons[winCombinations[i][2]]
        ) {
          result = 'Winner: ' + buttons[winCombinations[i][0]];

          // Update player win count
          if (buttons[winCombinations[i][0]] === 'X') {
            playerXWins++;
          } else {
            playerOWins++;
          }
          break;
        }
      }
    }
  }

  function undo() {
    if (previousStates.length > 0) {
      // Restore the previous state
      buttons = previousStates.pop();
      result = null;
      turn = turn == 'X' ? 'O' : 'X';
    }
  }

  function restart() {
    buttons = new Array(9).fill(null);
    result = null;
    turn = 'X';
  }
</script>

<section>
  <h1>My Tic-Tac-Toe</h1>

  <div class="board">
    {#if !result}
    <div class="grid">
      {#each buttons as button, index}
        <button
          on:click={() => setValue(index)}
          disabled={button !== null}
        >
          { button || '' }
        </button>
      {/each}
    </div>
    {:else}
    {#if result}
    <p>{result}</p>
  {/if}

    <div>
      <p>Player X Wins: {playerXWins}</p>
      <p>Player O Wins: {playerOWins}</p>
    </div>
    <button on:click={undo} disabled={result !== null}>
      Undo
    </button>
    <button on:click={restart}>
      Restart
    </button>
    {/if}
  </div>
</section>

<style>
  section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 0.6;
    background-color: #f7f7f7;
    padding: 2rem;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  h1 {
    width: 100%;
    font-size: 2rem;
    text-align: center;
    color: #333;
    margin-bottom: 1rem;
  }

  .board {
    background-color: #e0e0e0;
    border-radius: 10px;
    padding: 1rem;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(3, 3rem);
    grid-template-rows: repeat(3, 3rem);
    grid-gap: 0.3rem;
  }

  button {
    font-size: 1.2rem;
    font-family: "Arial", sans-serif;
    padding: 0.5rem 1rem;
    background-color: #3498db; 
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  button:hover {
    background-color: #2980b9; 
  }

</style>
