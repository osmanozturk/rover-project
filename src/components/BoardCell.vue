<template>
    <span>
        <span v-if="roverFacing === null">
            <span style="margin: 5px">
                *
            </span>
        </span>
        <span v-else>
            <span v-if="roverFacing === 'N'" style="margin: 5px">↑</span>
            <span v-if="roverFacing === 'S'" style="margin: 5px">↓</span>
            <span v-if="roverFacing === 'E'" style="margin: 5px">→</span>
            <span v-if="roverFacing === 'W'" style="margin: 5px">←</span>
        </span>
    </span>
</template>

<script>
export default {
    name: 'BoardCell',
    props: {
        //null value means empty cell
        roverFacing: {
            type: String,
            default: null
        }
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
