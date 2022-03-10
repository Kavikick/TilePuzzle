<script lang="ts">
    export let pieces: number[],
        displaySize: number,
        boardVisible: boolean,
        boardWidth: number,
        newBoard: (boardWidth: number) => number[];

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
        }, 500);
    }
</script>

<div class="boardSettings">
    <div class="boardSize">
        Change board size
        <button on:click={() => changeBoardSize("+")}>+</button>
        <button on:click={() => changeBoardSize("-")}>-</button>
    </div>
    <div class="displaySize">
        <button
            on:click={() => {
                displaySize = displaySize + 50;
            }}>Zoom in</button
        >
        <button
            on:click={() => {
                displaySize = displaySize - 50;
            }}>Zoom out</button
        >
    </div>
</div>

<style>
    .boardSettings {
        margin: 10px;
    }
</style>
