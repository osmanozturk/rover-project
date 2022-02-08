<template>
    <div>
        <div style="padding-bottom: 20px;">Board Size</div>
        <div v-if="!boardSizeConfirmed">
            <span>
                <label for="board-width-input">Board Width:
                    <input id="board-width-input" v-model="boardWidth" :placeholder="pleaseEnterPlaceholder" type="number" min="0" style="width:5%; margin-right: 50px;">
                </label>
            </span>
            <span>
                <label for="board-height-input">Board Height:
                    <input id="board-height-input" v-model="boardHeight" :placeholder="pleaseEnterPlaceholder" type="number" min="0" style="width:5%;  margin-right: 50px;">
                </label>
            </span>

            <span>
                <button id="confirm-board-size" :placeholder="pleaseEnterPlaceholder" @click="confirmBoardSize()">Confirm</button>
            </span>
        </div>
        <div v-else style="margin-bottom: 30px;">
            <span>
                Board Width: {{boardWidth}}
            </span>
            <span>
                Board Height: {{boardHeight}}
            </span>
        </div>

        <div id="board" v-if="boardSizeConfirmed">
            <span v-for="(y, yindex) in board" :key="'boardcell-row-' + yindex">
                <span v-for="(element, xindex) in board[yindex]" :key="'boardcell-column-' + xindex">
                    <board-cell :roverFacing="board[yindex][xindex]"></board-cell>
                </span>
                <br>
            </span>
            
        </div>


    <div style="padding-bottom: 20px; margin-top: 20px">Rovers
            <div>
                <!-- User will see a list of rovers to be placed and can give initial positions and facings to them -->
                <rover-input :index="0" :boardHeight="Number(boardHeight)" :boardWidth="Number(boardWidth)" 
                @position-confirmed="rover => confirmRoverPosition(rover)">

                </rover-input>
                        
            </div>
    </div>

    </div>
</template>

<script>
import RoverInput from './RoverInput.vue'
import BoardCell from './BoardCell.vue'
export default {
    name: 'MarsRovers',
    props: {

    },
    components: {
        RoverInput,
        BoardCell
    },
    data() {
        return {
            boardSizeConfirmed: false,
            boardWidth: 0,
            boardHeight: 0,
            rovers: [],
            board: []
        }
    },
    created() {
        this.calculateBoard(); //initializing empty board upon creation
    },
    watch: {
        //this watcher will recalculate the board everytime a change happens with rovers
        rovers: {
            handler: () => {
                this.calculateBoard(true);
            },
            deep: true //will allow us to watch changes within the array
        }
    },
    computed: {
        pleaseEnterPlaceholder() {
        return "Please give appropriate input"
        }
    },
    methods: {
        confirmBoardSize() {
            if (this.boardWidth > 0 && this.boardHeight > 0) {
                this.calculateBoard(true);
                this.boardSizeConfirmed = true;
            } else {
                alert("Please enter appropriate values for the board size");
            }
            
            return this.boardSizeConfirmed;
        },        
        confirmRoverPosition(rover) {
            //experimenting with only one rover
            //todo add multiple rover capability
            console.log('rovercame', rover);
            this.rovers[0] = rover;
            this.calculateBoard(true);
        },
        calculateBoard(debug) {
            //initializing the the board with null values,
            this.board= new Array(Number(this.boardHeight)).fill().map(() => new Array(Number(this.boardWidth)).fill(null));

                
            if (debug) {
                console.log('calculating board', Number(this.boardHeight), Number(this.boardWidth));
                console.log('calculating board res', this.board);
            }

            this.rovers.forEach(rover => {
                console.log('roverreport', rover.y, rover.x);
                this.board[rover.y][rover.x] = rover.facing;
            });

            return this.board;
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
