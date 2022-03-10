<script lang="ts">
    export let gameIsWon: boolean, totalMoves: number;
</script>

{#if gameIsWon}
    <div class="won">
        {totalMoves}
    </div>
{:else}
    <div class="playing">Current Moves : {totalMoves}</div>
{/if}

<style>
    .playing {
        margin-bottom: 10px;
    }

    .won {
        margin-top: 145px;
        margin-bottom: 180px;
        font-size: xx-large;
        color: goldenrod;
    }
</style>
