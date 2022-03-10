<script lang="ts">
    export let pieces: number[],
        displaySize: number,
        boardVisible: boolean,
        boardWidth: number,
        newBoard: (boardWidth: number) => number[],
        totalMoves: number,
        gameIsWon: boolean,
        displaySizeMemory;

    function changeBoardSize(operation: string) {
        boardVisible = false;
        setTimeout(() => {
            if (operation === "-") {
                boardWidth--;
            } else {
                boardWidth++;
            }
            pieces = newBoard(boardWidth);
            boardVisible = true;
            totalMoves = 0;
        }, 500);
    }
</script>

<div class="boardSettings">
    {#if !gameIsWon}
        <div class="boardSize">
            Change board size
            <button on:click={() => changeBoardSize("+")}>+</button>
            <button on:click={() => changeBoardSize("-")}>-</button>
        </div>
        <div class="displaySize">
            <button
                on:click={() => {
                    displaySize = displaySize + 100;
                }}>Zoom in</button
            >
            <button
                on:click={() => {
                    displaySize = displaySize - 100;
                }}>Zoom out</button
            >
        </div>
    {/if}

    <button
        on:click={() => {
            pieces = newBoard(boardWidth);
            totalMoves = 0;
            gameIsWon = false;
            displaySize = displaySizeMemory;
        }}>Reset game</button
    >
</div>

<style>
    .boardSettings {
        margin: 10px;
    }
</style>
