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
                <span>
                    Rover # {{index}}
                </span>
                <span>
                    <label :for="'rover-x-input-'+index"> X:
                        <input :id="'rover-x-input-'+index" type="number" min="0" :max="boardWidth" :placeholder="pleaseEnter">
                    </label>
                </span>
                <span>
                    <label :for="'rover-y-input-'+index"> Y:
                        <input :id="'rover-y-input-'+index" type="number" min="0" :max="boardHeight" :placeholder="pleaseEnter">
                    </label>
                </span>
                <span>
                    <label :for="'rover-facing-input-'+index"> Facing:
                        <select :id="'rover-facing-input-'+index" :placeholder="pleaseEnter">
                            <option value="N">North</option>
                            <option value="S">South</option>
                            <option value="E">East</option>
                            <option value="W">West</option>
                        </select>
                    </label>
                </span>
                <span>
                    <button>
                        Confirm
                    </button>
                </span>
            </div>
        </div>
    </div>



        <span>
            <button :placeholder="pleaseEnter">Confirm</button>
        </span>


    </div>
</template>

<script>
export default {
    name: 'MarsRovers',
    props: {

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
