<script lang="ts">
    import type { Tweened } from "svelte/motion";

    export let pieces: number[],
        displaySize: Tweened<number>,
        boardVisible: boolean,
        boardWidth: number,
        newBoard: (boardWidth: number) => number[],
        totalMoves: number,
        gameIsWon: boolean,
        displaySizeMemory: number,
        boardGap: number;

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
                    $displaySize = $displaySize + 100;
                }}>Zoom in</button
            >
            <button
                on:click={() => {
                    $displaySize = $displaySize - 100;
                }}>Zoom out</button
            >
        </div>
        <button
            on:click={() => {
                pieces = pieces.sort((a, b) => {
                    return a - b;
                });
                gameIsWon = true;
            }}>Solve for me!</button
        >
    {/if}

    <button
        on:click={() => {
            pieces = newBoard(boardWidth);
            totalMoves = 0;
            boardGap = 1;
            gameIsWon = false;
            $displaySize = displaySizeMemory;
        }}>Reset game</button
    >
</div>

<style>
    .boardSettings {
        margin: 10px;
    }
</style>
