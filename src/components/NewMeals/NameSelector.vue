<template>
  <div id="name-selector">
      Name:
      <div id="name">
          <input type="text" v-model="name" @change="changeHandler()">
      </div>
  </div>
</template>

<script>
    import EventBus from "../../event-bus.js";

    export default {
        name: 'NameSelector',
        props: ["errorReporter"],
        data() {
            return {
                name : ""
            }
        },
        mounted() {
            EventBus.$on("NameSelector.validateName", e => {
                //console.log("Validating Name..." + e);
                if (e == null) {
                    console.log("");
                }
                if (this.name === "") {
                    EventBus.$emit((this.errorReporter + "Error"), "Choose a name for your meal");
                } else {
                    EventBus.$emit("ValidationReturn", "Name," + this.name);
                }
            });
        },
        methods : {
            changeHandler : function () {
                //console.log("New name: " + this.name);
            }
        }
    }
</script>

<style>
    #name {
        text-align: center;
        padding: 20px;
    }
</style>
