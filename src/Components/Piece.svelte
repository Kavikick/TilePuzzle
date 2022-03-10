<script lang="ts">
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let x: number,
        y: number,
        pieceClass: string,
        ID: number,
        pictureWidth: number;

    function clickEvent() {
        dispatch("clickEvent", ID);
    }
</script>

<div
    style="background-position: {-pictureWidth * x}px {-pictureWidth *
        y}px; width: {pictureWidth}px; height: {pictureWidth}px;"
    class={pieceClass}
    on:click={clickEvent}
/>

<style>
    div.picturePiece {
        background-image: url("/download.jpeg");
        background-size: 600px 600px;
    }

    div.blackPiece {
        background-color: black;
    }
</style>
