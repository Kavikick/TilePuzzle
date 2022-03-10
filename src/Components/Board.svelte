<script lang="ts">
    import Piece from "./Piece.svelte";
    import BoardSettings from "./BoardSettings.svelte";
    import MoveCounter from "./MoveCounter.svelte";
    import { flip } from "svelte/animate";
    import { quintOut, cubicOut } from "svelte/easing";
    import { fade } from "svelte/transition";
    import { tweened } from "svelte/motion";

    let displaySizeMemory = 550;
    let boardGap = 1;
    let transitionLength = 1;
    let boardWidth = 4;
    let pieces = newBoard(boardWidth);
    let boardVisible = true;
    let totalMoves = 0;
    let gameIsWon = false;

    let displaySize = tweened(550, {
        duration: 400,
        easing: cubicOut,
    });
    $: adjustedDisplaySize = gameIsWon
        ? $displaySize
        : $displaySize + boardGap * 3; // to account for gap

    function newBoard(boardWidth: number) {
        return [...Array(boardWidth * boardWidth).keys()].sort(
            () => Math.random() - 0.5
        );
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
                gameIsWon = true;
            }
        }
    }

    function handleGameWin() {
        boardGap = 0;
        displaySizeMemory = $displaySize;
        $displaySize = 300;
    }
    $: gameIsWon
        ? setTimeout(() => {
              handleGameWin();
          }, 400)
        : {};

    function checkWin(): boolean {
        const key = [...pieces.keys()];
        for (let i = 0; i < pieces.length; i++) {
            if (pieces[i] !== key[i]) {
                return false;
            }
        }
        return true;
    }
</script>

<div class="gameScreen">
    {#if gameIsWon}
        <div
            class="background"
            transition:fade={{ duration: transitionLength * 1000 }}
        />
    {/if}
    <div class="foreground">
        <MoveCounter {gameIsWon} {totalMoves} {transitionLength} />
        <div
            class="board"
            style="grid-template-columns: {'auto '.repeat(boardWidth)}; 
            width: {adjustedDisplaySize}px; 
            height: {adjustedDisplaySize}px;
            gap: {boardGap}px;
            {gameIsWon ? 'border: 3px black solid' : ''}"
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
                            displaySize={$displaySize}
                            pictureWidth={$displaySize / boardWidth}
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
        <BoardSettings
            bind:pieces
            {displaySize}
            bind:boardVisible
            bind:boardWidth
            {newBoard}
            bind:totalMoves
            bind:gameIsWon
            {displaySizeMemory}
            bind:boardGap
        />
    </div>
</div>

<style>
    .board {
        background-color: aqua;
        display: grid;
        grid-auto-columns: auto;
        gap: 1px;
        border-radius: 1px;
    }

    .gameScreen {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .background {
        position: absolute;
        background-image: url("/frame.png");
        background-size: cover;
        height: 931px;
        width: 600px;
        z-index: 1;
    }

    .foreground {
        z-index: 2;
    }
</style>
