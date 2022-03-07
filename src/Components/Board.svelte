<script lang="ts">
    import Piece from "./Piece.svelte";

    let boardWidth = 4;
    let pieces = [...Array(boardWidth * boardWidth).keys()];

    function movePiece(ID: number) {
        // get neighbor IDs
        let IDs = [];
        IDs.push(ID - boardWidth); // above
        IDs.push(ID + boardWidth); // above
        IDs.push(ID - boardWidth); // above
        IDs.push(ID + boardWidth); // above

        // Lint for out of bounds
        IDs.filter((neighborID) => {
            if (neighborID <= 0) {
                return false;
            } else if (neighborID > boardWidth * boardWidth - 1) {
                return false;
            } else if (Math.floor(ID / 4) === Math.floor(neighborID / 4)) {
                return false;
            } else {
                return true;
            }
        });

        IDs.forEach((neighborID) => {});
        // access DOM to find out if one of the neighbors is the black square
        // swap the neighbor and piece's ID if the neighbor is the black square
    }
</script>

<div class="board">
    {#each pieces as ID (ID)}
        <Piece
            x={ID % 4}
            y={Math.floor(ID / 4)}
            pieceClass={ID === boardWidth * boardWidth - 1
                ? "blackPiece"
                : "picturePiece"}
            on:click={movePiece}
        />
    {/each}
</div>

<style>
    .board {
        background-color: aqua;
        width: 600px;
        height: 600px;
        display: grid;
        grid-auto-columns: auto;
        grid-template-columns: auto auto auto auto;
        gap: 1px;
    }
</style>
