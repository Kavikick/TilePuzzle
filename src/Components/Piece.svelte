<script lang="ts">
    export let x: number, y: number, pieceClass: string;
</script>

<div
    style="background-position: {-150 * x}px {-150 * y}px;"
    class={pieceClass}
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
