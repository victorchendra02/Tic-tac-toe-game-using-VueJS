<script setup>
import Board from "./components/Board.vue";
import Score from "./components/Score.vue";
</script>

<template>
    <!-- <div class="vh wrn3 p-5"> -->
    <div class="mt-4 mb-4">
        <h1 class="text-center m-0 fw-bold text-color-white">Tic Tac Toe</h1>
    </div>

    <div class="flex py-2 justify-content-space-evenly" style="height: auto">
        <Board />
        <Score />
    </div>
    <!-- </div> -->
</template>

<style scoped>
.justify-content-space-evenly {
    justify-content: space-evenly;
}
.flex {
    display: flex;
}
.aaa {
    background-color: rgb(162, 162, 162);
}
.wrn1 {
    background-color: brown;
}
.wrn2 {
    background-color: blue;
}
.wrn3 {
    background-color: rgb(40, 211, 71);
}
.vh {
    height: 100vh;
}
</style>
