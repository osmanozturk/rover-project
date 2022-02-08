<template>
    <div>
        <span>
            Command #{{index}}
        </span>
        <span>
            <label :for="'command-input-'+index"> Command: 
                <input :id="'command-input-'+index" v-model="command"  type="text">
            </label>
        </span>
        <span>
            <button @click="sendCommand()">
                Send
            </button>
        </span>
    </div>
</template>

<script>
export default {
    name: 'CommandInput',
    props: {
        index: Number //rover number
    },
    data() {
        return {
           command: ''
        }
    },
    computed: {
    },
    methods: {
        sendCommand() {
            //todo input validation
            //removing characters other than letters
            //and making them lowercase
            let loweredCommand = this.command.replace(/[^A-Za-z]/g, '').toLowerCase();
            return this.$emit('rover-command-sent', {
                index: this.index,
                command: loweredCommand
            });
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
