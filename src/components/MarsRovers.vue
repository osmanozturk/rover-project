<template>
    <div>
        <div style="padding-bottom: 20px;">Board Size</div>
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


    <div style="padding-bottom: 20px; margin-top: 20px">Rovers
        <div v-for="(rover, index) in rovers" :key="'rover'+index" style="margin-top: 10px">
            <div>
                <!-- User will see a list of rovers to be placed and can give initial positions and facings to them -->
                <rover-input :index="index">

                </rover-input>
                        
            </div>
        </div>
    </div>



        <span>
            <button :placeholder="pleaseEnter">Confirm</button>
        </span>


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
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
