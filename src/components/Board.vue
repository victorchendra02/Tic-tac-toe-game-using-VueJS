<!-- align-items-center -->
<!-- align-middle -->
<!-- justify-content-center -->
<template>
    <div class="flex flex-wrap box-shdw" style="width: 540px; height: 540px">
        <div v-for="(n, index) in table" :key="index">
            <div class="square" :class="aap" v-if="table[index] === null && is_end === false" @click="clicked(index)"></div>
            <div class="square-disabled" v-if="table[index] === null && is_end === true"></div>

            <div class="square-X biru" v-if="table[index] === 'X'"></div>
            <div class="square-O merah" v-if="table[index] === 'O'"></div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Board",
    data() {
        return {
            table: [null, null, null, null, null, null, null, null, null],
            move_count: 0,
            is_end: false,
            Xwin: undefined,
            Owin: undefined,
            draw: undefined,
            total_game: undefined,
            winner: "",
            Oturn: false,
        };
    },
    computed: {
        aap() {
            return {
                O: this.Oturn === true, // apply CSS class "O" if (this.Oturn === true)
            };
        },
    },

    methods: {
        clicked(index) {
            if (this.is_end === false) {
                if (this.move_count % 2 === 0) {
                    this.table[index] = "X";
                    this.Oturn = true;
                } else {
                    this.table[index] = "O";
                    this.Oturn = false;
                }
                this.move_count += 1;

                // printing
                console.log(`${this.table[0]} ${this.table[1]} ${this.table[2]}`);
                console.log(`${this.table[3]} ${this.table[4]} ${this.table[5]}`);
                console.log(`${this.table[6]} ${this.table[7]} ${this.table[8]}`);
                console.log(`__________________\n\n`);

                // Checking
                if (this.move_count >= 5) {
                    this.checking();
                    if (this.is_end === true) {
                        this.total_game = this.Xwin + this.Owin + this.draw;
                    }
                }

                if (this.move_count == 9 && this.is_end === false) {
                    this.is_end = true;
                    this.draw += 1;
                    this.winner = "draw";

                    this.total_game = this.Xwin + this.Owin + this.draw;
                    window.sessionStorage.setItem("draw", this.draw);
                }
                this.to_ScoreBoard();
            }
        },
        to_ScoreBoard() {
            this.emitter.emit("score", this.$data);
        },
        restart_game() {
            this.table = [null, null, null, null, null, null, null, null, null];
            this.move_count = 0;
            this.is_end = false;
            this.winner = "";
            this.Oturn = false;
            this.to_ScoreBoard();
            console.log("Game Restarted!");
        },
        reset_score() {
            if (this.Xwin === 0 && this.Owin === 0 && this.draw === 0) {
                this.restart_game();

                console.log("Score already resetted!");
                setTimeout(function () {
                    alert("Nothing to reset. Score already resetted!");
                }, 150);

                return;
            }

            window.sessionStorage.setItem("Xwin", 0);
            window.sessionStorage.setItem("Owin", 0);
            window.sessionStorage.setItem("draw", 0);

            this.Xwin = 0;
            this.Owin = 0;
            this.draw = 0;

            this.emitter.emit("score", this.$data);
            this.restart_game();

            console.log("Score Resetted!");
            setTimeout(function () {
                alert("Score has been reset!");
            }, 100);
        },
        checking() {
            // Rows check
            for (let i = 0; i < 7; i += 3) {
                if (this.table[i] === "X" && this.table[i + 1] === "X" && this.table[i + 2] === "X") {
                    console.log("Player X win\n\n\n");
                    this.winner = "X";

                    this.is_end = true;
                    this.Xwin += 1;
                    window.sessionStorage.setItem("Xwin", this.Xwin);
                    return;
                } else if (this.table[i] === "O" && this.table[i + 1] === "O" && this.table[i + 2] === "O") {
                    console.log("Player O win\n\n\n");
                    this.winner = "O";

                    this.is_end = true;
                    this.Owin += 1;
                    window.sessionStorage.setItem("Owin", this.Owin);
                    return;
                }
            }

            // Columns check
            for (let i = 0; i < 4; i++) {
                if (this.table[i] === "X" && this.table[i + 3] === "X" && this.table[i + 6] === "X") {
                    console.log("Player X win\n\n\n");
                    this.winner = "X";

                    this.is_end = true;
                    this.Xwin += 1;
                    window.sessionStorage.setItem("Xwin", this.Xwin);
                    return;
                } else if (this.table[i] === "O" && this.table[i + 3] === "O" && this.table[i + 6] === "O") {
                    console.log("Player O win\n\n\n");
                    this.winner = "O";

                    this.is_end = true;
                    this.Owin += 1;
                    window.sessionStorage.setItem("Owin", this.Owin);
                    return;
                }
            }

            // Vertical check
            // y = x
            if (this.table[0] === "X" && this.table[4] === "X" && this.table[8] === "X") {
                console.log("Player X win\n\n\n");
                this.winner = "X";

                this.is_end = true;
                this.Xwin += 1;
                window.sessionStorage.setItem("Xwin", this.Xwin + 1);
                return;
            }
            if (this.table[0] === "O" && this.table[4] === "O" && this.table[8] === "O") {
                console.log("Player O win\n\n\n");
                this.winner = "O";

                this.is_end = true;
                this.Owin += 1;
                window.sessionStorage.setItem("Owin", this.Owin);
                return;
            }

            // y = -x
            if (this.table[2] === "X" && this.table[4] === "X" && this.table[6] === "X") {
                console.log("Player X win\n\n\n");
                this.winner = "X";

                this.is_end = true;
                this.Xwin += 1;
                window.sessionStorage.setItem("Xwin", this.Xwin + 1);
                return;
            }
            if (this.table[2] === "O" && this.table[4] === "O" && this.table[6] === "O") {
                console.log("Player O win\n\n\n");
                this.winner = "O";

                this.is_end = true;
                this.Owin += 1;
                window.sessionStorage.setItem("Owin", this.Owin);
                return;
            }
        },
    },

    created() {
        let Xwin_ = parseInt(window.sessionStorage.getItem("Xwin"));
        let Owin_ = parseInt(window.sessionStorage.getItem("Owin"));
        let draw_ = parseInt(window.sessionStorage.getItem("draw"));

        if (isNaN(Xwin_) || isNaN(Owin_) || isNaN(draw_)) {
            this.Xwin = 0;
            this.Owin = 0;
            this.draw = 0;

            window.sessionStorage.setItem("Xwin", this.Xwin);
            window.sessionStorage.setItem("Owin", this.Owin);
            window.sessionStorage.setItem("draw", this.draw);

            this.total_game = 0;
        } else {
            this.Xwin = Xwin_;
            this.Owin = Owin_;
            this.draw = draw_;
            this.total_game = this.Xwin + this.Owin + this.draw;
        }

        this.emitter.on("restart_game", (status) => {
            this.restart_game();
        });
        this.emitter.on("reset_score", (status) => {
            this.reset_score();
        });
    },
};
</script>

<style scoped>
.square-X,
.square-O,
.square,
.square-disabled {
    width: 180px;
    height: 180px;
}
/*  */
/*  */
/*  */
/*  */

.square {
    cursor: pointer;
    background-color: #212121;
}
.square:hover {
    background-color: #008ddb;
}
.O:hover {
    background-color: #e4005a;
}

.square-disabled {
    background-color: #212121;
}

/*  */
/*  */
/*  */
/*  */
.my-border {
    border: 5px solid #000000;
}
.ppp {
    width: 300px;
}
.wrn1 {
    background-color: rgb(0, 0, 0);
}
.wrn2 {
    background-color: rgb(126, 169, 97);
}
</style>
