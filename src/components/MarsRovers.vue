<template>
    <div>
        <div style="padding-bottom: 20px;">Board Size</div>
        <div v-if="!boardSizeConfirmed">
            <span>
                <label for="board-width-input">Board Width:
                    <input id="board-width-input" v-model="boardWidth" :placeholder="pleaseEnter" type="number" min="0" style="width:5%; margin-right: 50px;">
                </label>
            </span>
            <span>
                <label for="board-height-input">Board Height:
                    <input id="board-height-input" v-model="boardHeight" :placeholder="pleaseEnter" type="number" min="0" style="width:5%;  margin-right: 50px;">
                </label>
            </span>

            <span>
                <button id="confirm-board-size" :placeholder="pleaseEnter" @click="confirmBoardSize()">Confirm</button>
            </span>
        </div>
        <div v-else>
            <span>
                Board Width: {{boardWidth}}
            </span>
            <span>
                Board Height: {{boardHeight}}
            </span>
        </div>

    <div style="padding-bottom: 20px; margin-top: 20px">Rovers
        <div v-for="(rover, index) in rovers" :key="'rover'+index" style="margin-top: 10px">
            <div>
                <!-- User will see a list of rovers to be placed and can give initial positions and facings to them -->
                <rover-input :index="index">

                </rover-input>
                        
            </div>
        </div>
    </div>

    </div>
</template>

<script>
import RoverInput from './RoverInput.vue'
export default {
    name: 'MarsRovers',
    props: {

    },
    components: {
        RoverInput
    },
    data() {
        return {
            boardSizeConfirmed: false,
            boardWidth: null,
            boardHeight: null,
            rovers: [{
                x: 0,
                y: 0,
                facing: "N"
            }]
        }
    },
    computed: {
        pleaseEnter() {
        return "Please give appropriate input"
        }
    },
    methods: {
        confirmBoardSize() {
            if (this.boardWidth > 0 && this.boardHeight > 0) {
                this.boardSizeConfirmed = true;
            } else {
                alert("Please enter appropriate values for the board size");
            }
            
            return this.boardSizeConfirmed;
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
