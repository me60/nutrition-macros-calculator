<template>
  <div id="error-footer">
      <div v-bind:style="{ color : activeColor }"> {{ errorText }} </div>
  </div>
</template>

<script>
    import EventBus from "../../event-bus.js";

    export default {
        name: 'ErrorFooter',
        props: ["setName"],
        data() {
            return {
                errorText : "No errors yet :)",
                activeColor : "green"
            }
        },
        mounted() {
            EventBus.$on((this.setName + "Error"), e => {
                this.activeColor = "red";
                this.errorText = e;
            });
            EventBus.$on((this.setName + "Success"), e => {
                this.activeColor = "green";
                this.errorText = e;
            });
        }
    }
</script>

<style>
    #error-footer {
        background-color: black;
        color: red;
        text-align: center;
    }
</style>
