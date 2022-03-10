<script lang="ts">
    import Piece from "./Piece.svelte";
    import BoardSettings from "./BoardSettings.svelte";
    import MoveCounter from "./MoveCounter.svelte";
    import { flip } from "svelte/animate";
    import { quintOut } from "svelte/easing";
    import { fade } from "svelte/transition";

    let displaySize = 550;
    let boardWidth = 4;
    let pieces = newBoard(boardWidth);
    let boardVisible = true;
    let totalMoves = 0;
    let boardGap = 2;
    let gameIsWon = false;

    function newBoard(boardWidth: number) {
        if (false) {
            return [...Array(boardWidth * boardWidth).keys()].sort(
                () => Math.random() - 0.5
            );
        } else {
            return [...Array(boardWidth * boardWidth).keys()];
        }
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
            totalMoves++;
            if (checkWin()) {
                handleGameWin();
            }
        }
    }

    function handleGameWin() {
        gameIsWon = true;
        boardGap = 0;
        displaySize = 300;
    }

    function checkWin(): boolean {
        const key = [...pieces.keys()];
        for (let i = 0; i < pieces.length; i++) {
            if (pieces[i] !== key[i]) {
                return false;
            }
        }
        return true;
    }

    // handleGameWin();
</script>

<div class="gameScreen">
    <MoveCounter {gameIsWon} {totalMoves} />
    <div
        class="board"
        style="grid-template-columns: {'auto '.repeat(
            boardWidth
        )}; width: {displaySize}px; height: {displaySize}px;
         gap: {boardGap}px;"
    >
        {#if boardVisible}
            {#each pieces as ID (ID)}
                <div
                    animate:flip={{
                        delay: 10,
                        duration: 400,
                        easing: quintOut,
                    }}
                    transition:fade
                >
                    <Piece
                        {displaySize}
                        pictureWidth={displaySize / boardWidth}
                        {ID}
                        x={ID % boardWidth}
                        y={Math.floor(ID / boardWidth)}
                        isBlackPiece={gameIsWon
                            ? false
                            : ID === boardWidth * boardWidth - 1}
                        on:clickEvent={gameIsWon ? () => {} : movePiece}
                    />
                </div>
            {/each}
        {/if}
    </div>
</div>
{#if !gameIsWon}
    <BoardSettings
        bind:pieces
        bind:displaySize
        bind:boardVisible
        bind:boardWidth
        {newBoard}
        bind:totalMoves
    />
{/if}

<style>
    .board {
        background-color: aqua;
        display: grid;
        grid-auto-columns: auto;
        gap: 1px;
        border: 3px black solid;
        border-radius: 1px;
    }

    .gameScreen {
        background-image: url("/frame.png");
        background-size: cover;
        height: 931px;
        width: 600px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
</style>
