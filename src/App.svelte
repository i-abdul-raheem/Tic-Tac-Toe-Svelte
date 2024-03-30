<script>
  let next = "O";
  let marks = Array(9).fill(".");
  let winnerMessage = "";

  const resetGame = () => {
    next = "O";
    marks = Array(9).fill(".");
    setTimeout(() => {
      winnerMessage = "";
    }, 2000);
  };

  const calculateWinner = () => {
    const lines = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];
    for (const [a, b, c] of lines) {
      if (marks[a] !== "." && marks[a] === marks[b] && marks[a] === marks[c]) {
        return marks[a];
      }
    }
    if (!marks.includes(".")) {
      resetGame();
      next = "X";
    }
    return null;
  };

  const handleClick = (index) => {
    if (marks[index] !== "." || winnerMessage) return;
    marks[index] = next;
    const winner = calculateWinner();
    if (winner) {
      winnerMessage = `${winner} wins`;
      resetGame();
      return;
    }
    next = next === "O" ? "X" : "O";
  };
</script>

<main>
  {#if winnerMessage}
    <p class="winner">{winnerMessage}</p>
  {:else}
    <div class="board">
      {#each marks as mark, index (index)}
        <button class="box" on:click={() => handleClick(index)}>
          <span>{mark}</span>
        </button>
      {/each}
    </div>
  {/if}
</main>

<style>
  main {
    text-align: center;
  }

  .board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    grid-gap: 5px;
    background-color: #444;
    padding: 5px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }

  .box {
    border: none;
    background-color: #ddd;
    color: #333;
    font-size: 3em;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: background-color 0.3s ease;
    border-radius: 5px;
  }

  .box:hover {
    background-color: #ccc;
  }

  .box span {
    pointer-events: none;
  }

  .winner {
    font-size: 2em;
    font-weight: bold;
    color: #f44336;
    margin-top: 20px;
    text-align: center;
  }
</style>
