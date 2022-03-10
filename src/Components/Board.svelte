<script lang="ts">
    import Piece from "./Piece.svelte";
    import { flip } from "svelte/animate";
    import { quintOut } from "svelte/easing";
    import { fade } from "svelte/transition";

    let displaySize = 550;
    let boardWidth = 4;
    let pieces = newBoard(boardWidth);
    let boardVisible = true;

    function newBoard(boardWidth: number) {
        return [...Array(boardWidth * boardWidth).keys()].sort(
            () => Math.random() - 0.5
        );
    }

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

<div class="boardSettings">
    <div class="boardSize">
        Change board size
        <button on:click={() => changeBoardSize("+")}>+</button>
        <button on:click={() => changeBoardSize("-")}>-</button>
    </div>
    <div class="displaySize">
        <button
            on:click={() => {
                displaySize += 50;
            }}>Zoom in</button
        >
        <button
            on:click={() => {
                displaySize -= 50;
            }}>Zoom out</button
        >
    </div>
</div>
<div
    class="board"
    style="grid-template-columns: {'auto '.repeat(
        boardWidth
    )}; width: {displaySize}px; height: {displaySize}px;"
>
    {#if boardVisible}
        {#each pieces as ID (ID)}
            <div
                animate:flip={{ delay: 10, duration: 400, easing: quintOut }}
                transition:fade
            >
                <Piece
                    {displaySize}
                    pictureWidth={displaySize / boardWidth}
                    {ID}
                    x={ID % boardWidth}
                    y={Math.floor(ID / boardWidth)}
                    isBlackPiece={ID === boardWidth * boardWidth - 1}
                    on:clickEvent={movePiece}
                />
            </div>
        {/each}
    {/if}
</div>

<style>
    .board {
        background-color: aqua;
        display: grid;
        grid-auto-columns: auto;
        gap: 1px;
    }

    .boardSettings {
        margin: 10px;
    }
</style>
