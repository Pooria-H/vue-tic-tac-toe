<template>
    <button @click="onCellClick(number)" type="button" class="cell" :class="'cell-' + number" :disabled="disabledCell">{{ filledData }}</button>
</template>

<script>
export default {
    name: 'cell',
    props: {
        bus: {
            type: Object,
            required: true,
        },
        number: {
            type: Number,
            required: true,
        },
        activePlayer: {
            type: String,
            required: false,
        },
        isGameFinished: {
            type: Boolean,
            required: true,
        }
    },
    methods: {
        onCellClick(number) {
            this.$emit('cellClicked', number);
            this.filledData = this.activePlayer;
            this.readOnly = true;
        },
        clearCell() {
            this.filledData = '';
            this.readOnly = false;
        },
    },
    computed: {
        disabledCell() {
            if (this.readOnly || this.isGameFinished) {
                return true;
            }
            return false;
        }
    },
    mounted() {
        this.bus.$on('clearCell', this.clearCell);
    },
    data() {
        return {
            filledData: '',
            readOnly: false,
        };
    },
}
</script>

<style lang="scss">
    .cell {
        display: block;
        width: 64px;
        height: 64px;
        cursor: pointer;
        background-color: transparent;
        border: none;
        font-size: 2em;
        border: 1px solid gray;
        &:hover {
            background-color: rgba(255, 255, 255, .3);
            box-shadow: 0 0 32px 0 rgba(255, 255, 255, .7);
        }
        &:disabled {
            background-color: transparent;
            cursor: initial;
            box-shadow: none;
        }
    }
    .cell-1, .cell-2, .cell-3 {
        border-top: none;
    }
    .cell-7, .cell-8, .cell-9 {
        border-bottom: none;
    }
    .cell-1, .cell-4, .cell-7 {
        border-left: none;
    }
    .cell-3, .cell-6, .cell-9 {
        border-right: none;
    }
</style>


