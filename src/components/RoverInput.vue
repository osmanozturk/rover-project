<template>
    <div>
        <span>
            Rover # {{index}}
        </span>
        <span>
            <label :for="'rover-x-input-'+index"> X:
                <input :id="'rover-x-input-'+index" v-model="x"  type="number" min="0" :max="boardWidth">
            </label>
        </span>
        <span>
            <label :for="'rover-y-input-'+index"> Y:
                <input :id="'rover-y-input-'+index" v-model="y" type="number" min="0" :max="boardHeight">
            </label>
        </span>
        <span>
            <label :for="'rover-facing-input-'+index"> Facing:
                <select :id="'rover-facing-input-'+index" v-model="facing">
                    <option value="N">North</option>
                    <option value="S">South</option>
                    <option value="E">East</option>
                    <option value="W">West</option>
                </select>
            </label>
        </span>
        <span>
            <button @click="confirmPosition()">
                Confirm
            </button>
        </span>
    </div>
</template>

<script>
export default {
    name: 'RoverInput',
    props: {
        index: Number,

        boardHeight: {
            type: Number,
            default: null
        },
        boardWidth: {
            type: Number,
            default: null
        },

    },
    data() {
        return {
           x: 0,
           y: 0,
           facing: 'N'
        }
    },
    computed: {
        isPositionsValid() {
            //making sure the size is confirmed
            return  this.boardHeight !== null && this.boardWidth !== null 
            && (this.x >= 0 && this.x < this.boardWidth) && (this.y >= 0 && this.y < this.boardHeight);
        }
    },
    methods: {
        confirmPosition() {
            if (this.isPositionsValid) {
                this.$emit('position-confirmed', {
                    x: this.x,
                    y: this.y,
                    facing: this.facing
                })
            } else {
                alert("Please enter appropriate values for the rover position");
            }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
