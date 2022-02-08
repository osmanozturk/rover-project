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
            <span v-for="(y, yindex) in yReversedBoard" :key="'boardcell-row-' + yindex">
                <span v-for="(element, xindex) in y" :key="'boardcell-column-' + xindex">
                    <board-cell :roverFacing="element"></board-cell>
                </span>
                <br>
            </span>
            
        </div>


    <div style="padding-bottom: 20px; margin-top: 20px">Rover Positions
            <div>
                <!-- User will see a list of rovers to be placed and can give initial positions and facings to them -->
                <rover-input v-if="!roversInitialized[0]" :index="0" :boardHeight="Number(boardHeight)" :boardWidth="Number(boardWidth)" 
                @position-confirmed="rover => confirmRoverPosition(rover, 0)">
                </rover-input>

                <div v-else style="margin-top: 20px">
                    Rover #0 X: {{rovers[0].x}} Y: {{rovers[0].y}}  Facing:{{rovers[0].facing}}
                </div>

                        
            </div>
    </div>

    <div style="padding-bottom: 20px; margin-top: 20px">Commands
            <div style="margin-top: 20px;">
                <!-- User will see a list of rovers that are placed and can give commands to them -->
                <command-input 
                    :index="0" 
                    @rover-command-sent="commandObj => parseCommand(commandObj)">
                </command-input>
                        
            </div>
    </div>

    </div>
</template>

<script>
import RoverInput from './RoverInput.vue'
import CommandInput from './CommandInput.vue'
import BoardCell from './BoardCell.vue'
export default {
    name: 'MarsRovers',
    props: {

    },
    components: {
        RoverInput,
        CommandInput,
        BoardCell
    },
    data() {
        return {
            boardSizeConfirmed: false,
            boardWidth: 0,
            boardHeight: 0,
            rovers: [],
            board: [],
            roversInitialized: [false]
        }
    },
    created() {
        this.calculateBoard(); //initializing empty board upon creation
    },
    watch: {
        //this watcher will recalculate the board everytime a change happens with rovers
        rovers: {
            //reverted arrow function definition for proper 'this' context
            handler() {
                this.calculateBoard(true);
            },
            deep: true //will allow us to watch changes within the array
        }
    },
    computed: {
        allRoversInitialized() {
            //will be useful when we have multiple rovers
            return this.roversInitialized.every(isInitialized => isInitialized === true);
        },
        pleaseEnterPlaceholder() {
        return "Please give appropriate input"
        },
        //reverses the board array so that y+1 is to the north of y.
        yReversedBoard() {
            //slice creates a copy of the board so we don't mutate the board
            return this.board.slice(0).reverse();
        }
    },
    methods: {
        //sending command as object with index in case if I decide to send the command readily parsed as an array in the future
        //it will be cleaner
        parseCommand(commandObj) {
            let {index, command} = commandObj;
            let commandArr = command.split(''); //Splitting each character

            //only moving for now
            commandArr.forEach(cmd => {
                switch (cmd) {
                    case 'm':
                        this.move(index);
                        break;
                    case 'l':
                        this.turnNinetyDegrees(index, true);
                        break;
                    case 'r':
                        this.turnNinetyDegrees(index, false);
                        break;
                }               
            });
            
        },

        move(index) {
            let rover = this.rovers[index];

            //not doing anything if the move is invalid
            switch (rover.facing) {
                case 'N':
                    rover.y+1 < this.boardHeight ? rover.y++ : null;
                    break;
                case 'S':
                    rover.y-1 > 0 ? rover.y-- : null;
                    break;
                case 'E':
                    rover.x+1 < this.boardWidth ? rover.x++ : null;
                    break;
                case 'W':
                    rover.y-1 > 0 ? rover.x-- : null;
                    break;
            }
        },

        turnNinetyDegrees(index, counterClockwise) {
            //not doing anything if the turn is invalid
            let rover = this.rovers[index];
            switch (rover.facing) {
                case 'N':
                    rover.facing = counterClockwise ? 'W' : 'E'; 
                    break;
                case 'S':
                    rover.facing = counterClockwise ? 'E' : 'W'; 
                    break;
                case 'E':
                    rover.facing = counterClockwise ? 'N' : 'S'; 
                    break;
                case 'W':
                    rover.facing = counterClockwise ? 'S' : 'N'; 
                    break;
            }
        },
        confirmBoardSize() {
            //Todo input validation
            if (this.boardWidth > 0 && this.boardHeight > 0) {
                this.calculateBoard(true);
                this.boardSizeConfirmed = true;
            } else {
                alert("Please enter appropriate values for the board size");
            }
            
            return this.boardSizeConfirmed;
        },        
        confirmRoverPosition(rover, index) {
            //experimenting with only one rover
            //todo add multiple rover capability
            //todo input validation
            this.$set(this.rovers, index, rover);
            this.roversInitialized[index] = true;
            //Vue.set makes the rovers reactive so that our deep watcher can detect the changes and recalculate the board
        },
        calculateBoard(debug) {
            //initializing the the board with null values,
            this.board = new Array(Number(this.boardHeight)).fill().map(() => new Array(Number(this.boardWidth)).fill(null));

                
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
