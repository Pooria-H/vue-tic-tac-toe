<template>
    <div>
        <table>
            <tr>
                <td>
                    <cell @cellClicked="onCellClick" :bus="bus" :is-game-finished="isGameFinished" :active-player="activePlayer" :number="1"></cell>
                </td>
                <td>
                    <cell @cellClicked="onCellClick" :bus="bus" :is-game-finished="isGameFinished" :active-player="activePlayer" :number="2"></cell>
                </td>
                <td>
                    <cell @cellClicked="onCellClick" :bus="bus" :is-game-finished="isGameFinished" :active-player="activePlayer" :number="3"></cell>
                </td>
            </tr>
            <tr>
                <td>
                    <cell @cellClicked="onCellClick" :bus="bus" :is-game-finished="isGameFinished" :active-player="activePlayer" :number="4"></cell>
                </td>
                <td>
                    <cell @cellClicked="onCellClick" :bus="bus" :is-game-finished="isGameFinished" :active-player="activePlayer" :number="5"></cell>
                </td>
                <td>
                    <cell @cellClicked="onCellClick" :bus="bus" :is-game-finished="isGameFinished" :active-player="activePlayer" :number="6"></cell>
                </td>
            </tr>
            <tr>
                <td>
                    <cell @cellClicked="onCellClick" :bus="bus" :is-game-finished="isGameFinished" :active-player="activePlayer" :number="7"></cell>
                </td>
                <td>
                    <cell @cellClicked="onCellClick" :bus="bus" :is-game-finished="isGameFinished" :active-player="activePlayer" :number="8"></cell>
                </td>
                <td>
                    <cell @cellClicked="onCellClick" :bus="bus" :is-game-finished="isGameFinished" :active-player="activePlayer" :number="9"></cell>
                </td>
            </tr>
        </table>
        <div v-html="infoText" class="info"></div>
    </div>
</template>

<script>
import cell from './components/cell';

export default {
    name: 'board',
    components: {
        cell,
    },
    props: {
        bus: {
            type: Object,
            required: true,
        },
        firstPlayer: {
            type: String,
            required: true,
        },
        isGameFinished: {
            type: Boolean,
            required: true,
        },
    },
    methods: {
        onCellClick(cellNumber) {
            this.numberOfTurn += 1;
            this.data[cellNumber] = this.activePlayer;
            if (this.activePlayer === 'X') {
                this.activePlayer = 'O';
            } else {
                this.activePlayer = 'X';
            }
            this.updateInfoText(
                `
                <b>
                    <span class="upperCase">${this.activePlayer}</span>'s
                </b> turn.
            `
            );

            this.checkIfWeHaveAWinner();
        },
        checkIfWeHaveAWinner() {
            this.winPatterns.forEach((arr, index) => {
                this.tempCheckArr = [
                    this.data[arr[0]],
                    this.data[arr[1]],
                    this.data[arr[2]],
                ];

                if (this.tempCheckArr.every( (val, i, arr) => val === arr[0] ) && this.tempCheckArr[0] !== '') {
                    this.winner = this.tempCheckArr[0];
                    this.updateInfoText(
                        `
                        Player
                        ${this.winner}
                        is the winner!
                    `
                    );
                    this.$emit('gameFinished', this.winner);
                }
            });
        },
        resetBoard() {
            this.data = ['reserved', '', '', '', '', '', '', '', '', ''];
            this.tempCheckArr = [];
            this.numberOfTurn = 0;
            this.updateInfoText(
                `
                <b>
                    <span class="upperCase">${this.activePlayer}</span>'s
                </b> turn.
            `
            );
            this.winner = '';
            this.bus.$emit('clearCell');
        },
        updateInfoText(text) {
            this.infoText = text;
        },
    },
    data() {
        return {
            data: ['reserved', '', '', '', '', '', '', '', '', ''],
            tempCheckArr: [],
            winPatterns: [
                [1, 2, 3],
                [4, 5, 6],
                [7, 8, 9],
                [1, 4, 7],
                [2, 5, 8],
                [3, 6, 9],
                [1, 5, 9],
                [3, 5, 7],
            ],
            activePlayer: this.firstPlayer,
            numberOfTurn: 0,
            infoText: '',
            winner: '',
        };
    },
    created() {
        this.updateInfoText(
            `
            <b>
                <span class="upperCase">${this.activePlayer}</span>'s
            </b> turn.
        `
        );
    },
    mounted() {
        this.bus.$on('createNewGame', this.resetBoard);
    },
}
</script>

<style lang="scss">
    table {
        border-spacing: 0;
        margin: auto;
    }
    td {
        padding: 0;
    }
    .info {
        margin-top: 10px;
        font-size: 1.1em;
        font-weight: bold;
        color: #ff5b5b;
        text-align: center;
    }
</style>

