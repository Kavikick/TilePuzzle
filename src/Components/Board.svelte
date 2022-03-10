<script lang="ts">
    import Piece from "./Piece.svelte";
    import { flip } from "svelte/animate";
    import { quintOut } from "svelte/easing";

    let boardWidth = 4;
    let pieces = [...Array(boardWidth * boardWidth).keys()].sort(
        () => Math.random() - 0.5
    );

    function getNeighborIDs(ID: number): number[] {
        const pieceIndex = pieces.indexOf(ID);

        // get neighbor IDs and bounds check
        let neighborIndexes = [pieceIndex - 1, pieceIndex + 1].filter(
            (neighborIndex) => {
                return (
                    Math.floor(pieceIndex / boardWidth) ===
                    Math.floor(neighborIndex / boardWidth)
                );
            }
        );
        neighborIndexes = neighborIndexes
            .concat([pieceIndex - boardWidth, pieceIndex + boardWidth])
            .filter((neighborIndex) => {
                return (
                    neighborIndex >= 0 &&
                    neighborIndex < boardWidth * boardWidth
                );
            });

        // Convert indexes to IDs
        let neighborIDs = [];
        neighborIndexes.forEach((index) => {
            neighborIDs.push(pieces[index]);
        });
        return neighborIDs;
    }

    function movePiece(event: CustomEvent) {
        const pieceID = event.detail;
        const blackID = boardWidth * boardWidth - 1;
        const neighborIDs = getNeighborIDs(pieceID);
        if (neighborIDs.includes(blackID)) {
            let pieceIndex = pieces.indexOf(pieceID);
            let blackIndex = pieces.indexOf(blackID);
            pieces[pieceIndex] = blackID;
            pieces[blackIndex] = pieceID;
        }
    }
</script>

<div class="board">
    {#each pieces as ID (ID)}
        <div animate:flip={{ delay: 10, duration: 400, easing: quintOut }}>
            <Piece
                {ID}
                x={ID % boardWidth}
                y={Math.floor(ID / boardWidth)}
                pieceClass={ID === boardWidth * boardWidth - 1
                    ? "blackPiece"
                    : "picturePiece"}
                on:clickEvent={movePiece}
            />
        </div>
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
