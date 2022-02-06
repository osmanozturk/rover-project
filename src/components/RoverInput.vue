<template>
    <div>
        <span>
            Rover # {{index}}
        </span>
        <span>
            <label :for="'rover-x-input-'+index"> X:
                <input :id="'rover-x-input-'+index" type="number" min="0" :max="boardWidth">
            </label>
        </span>
        <span>
            <label :for="'rover-y-input-'+index"> Y:
                <input :id="'rover-y-input-'+index" type="number" min="0" :max="boardHeight">
            </label>
        </span>
        <span>
            <label :for="'rover-facing-input-'+index"> Facing:
                <select :id="'rover-facing-input-'+index">
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
</template>

<script>
export default {
    name: 'RoverInput',
    props: {
        index: Number,
        boardHeight: Number,
        boardWidth: Number
    },
    data() {
        return {
           
        }
    },
    computed: {

    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
