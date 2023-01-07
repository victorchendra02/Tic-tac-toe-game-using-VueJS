<template>
    <div class="bg-info text-center" style="width: 400px">
        <h2>Score Status</h2>
        <hr />

        Player X: {{ Xwin }} Win
        <br />
        Player O: {{ Owin }} Win
        <br />
        Draw: {{ draw }}
        <br />
        <br />
        winrate X: {{ ((Xwin / (Xwin + Owin + draw)) * 100).toFixed(0) }}% <br />
        winrate O: {{ ((Owin / (Xwin + Owin + draw)) * 100).toFixed(0) }}% <br />
        drawrate: {{ ((draw / (Xwin + Owin + draw)) * 100).toFixed(0) }}%
        <br />
        <br />
        <hr />
        <button type="button" class="btn btn-primary" @click="restart_game()">Restart Game</button><br /><br />
    </div>
</template>

<script>
export default {
    name: "Score",
    data() {
        return {
            Xwin: parseInt(window.localStorage.getItem("Xwin")),
            Owin: parseInt(window.localStorage.getItem("Owin")),
            draw: parseInt(window.localStorage.getItem("draw")),
        };
    },

    methods: {
        terimadata(datanya) {
            this.Xwin = datanya.Xwin;
            this.Owin = datanya.Owin;
            this.draw = datanya.draw;
        },
        restart_game() {
            this.emitter.emit("restart_game", true);
        },
    },

    created() {
        this.emitter.on("score", (transfered_data) => {
            let x = transfered_data;
            this.terimadata(x);
        });
    },
};
</script>

<style></style>
