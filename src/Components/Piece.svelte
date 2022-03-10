<script lang="ts">
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let x: number,
        y: number,
        ID: number,
        pictureWidth: number,
        isBlackPiece: Boolean,
        displaySize: number;

    function clickEvent() {
        dispatch("clickEvent", ID);
    }
</script>

{#if isBlackPiece}
    <div
        class="blackPiece"
        style="width: {pictureWidth}px; height: {pictureWidth}px;"
    />
{:else}
    <div
        style="background-position: {-pictureWidth * x}px {-pictureWidth *
            y}px; width: {pictureWidth}px; height: {pictureWidth}px; background-size: {displaySize}px {displaySize}px;"
        class={"picturePiece"}
        on:click={clickEvent}
    />
{/if}

<style>
    div.picturePiece {
        background-image: url("/download.jpeg");
    }

    div.blackPiece {
        background-color: black;
    }
</style>
