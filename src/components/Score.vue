<template>
    <div class="text-center buat-radius" style="width: 400px; background-color: #212121">
        <h2 class="fw-bold text-color-white py-3">Score Status</h2>

        <div class="flex">
            <div class="text-vertically-center biru" style="width: 50%; height: 150px">
                <div>
                    <i class="fas fa-times fs-1 pb-2"></i>
                    <br />
                    {{ Xwin }} Win <br />
                    Winrate {{ ((Xwin / (Xwin + Owin + draw)) * 100).toFixed(2) }}%
                </div>
            </div>
            <div class="text-vertically-center merah" style="width: 50%; height: 150px">
                <div>
                    <i class="far fa-circle fs-1 pb-2"></i>
                    <br />
                    {{ Owin }} Win <br />
                    Winrate {{ ((Owin / (Xwin + Owin + draw)) * 100).toFixed(2) }}%
                </div>
            </div>
        </div>

        <div class="text-vertically-center kuning flex-direction-column" style="width: 100%; height: 150px">
            <div class="fs-1">&half;</div>
            <br />
            {{ draw }} Draw<br />
            Drawrate {{ ((draw / (Xwin + Owin + draw)) * 100).toFixed(2) }}%
        </div>

        <div class="text-vertically-center" style="background-color: #fff; width: 100%; height: 75px">
            <span v-if="winner === ''">....</span>
            <span v-if="winner === 'X'">X Win the game</span>
            <span v-if="winner === 'O'">O Win the game</span>
            <span v-if="winner === 'draw'">It's Draw!</span>
        </div>

        <div>
            <br />
            <button type="button" class="btn btn-primary me-3" @click="restart_game()">Restart Game <i class="fas fa-redo-alt"></i></button>
            <button type="button" class="btn btn-danger" @click="reset_score()">Reset Score <i class="fas fa-redo-alt"></i></button>
        </div>
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
            winner: "",
        };
    },

    methods: {
        terimadata(datanya) {
            this.Xwin = datanya.Xwin;
            this.Owin = datanya.Owin;
            this.draw = datanya.draw;
            this.winner = datanya.winner;
        },
        restart_game() {
            this.emitter.emit("restart_game", true);
        },
        reset_score() {
            this.emitter.emit("reset_score", true);
        },
        whoiswin() {},
    },

    created() {
        this.emitter.on("score", (transfered_data) => {
            let x = transfered_data;
            this.terimadata(x);
        });
    },
};
</script>

<style scoped>
.buat-radius {
    border-radius: 8px;
    box-shadow: 0px 0px 40px -32px rgb(85, 85, 85);
}
</style>
