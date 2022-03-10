<script lang="ts">
    export let gameIsWon: boolean, totalMoves: number, transitionLength: number;
</script>

<div
    style="margin-bottom: {gameIsWon ? 180 : 10}px; 
        margin-top: {gameIsWon ? 150 : 0}px;
        {gameIsWon ? 'color: goldenrod; font-size: xx-large;' : ''}
        transition-duration: {transitionLength}s"
>
    {#if gameIsWon}
        {totalMoves}
    {:else}
        Current Moves : {totalMoves}
    {/if}
</div>

<style>
    div {
        transition: all;
        transition-timing-function: ease-in-out;
        transform: translate();
    }
</style>
