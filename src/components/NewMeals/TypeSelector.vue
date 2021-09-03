<template>
  <div id="type-selector">
      Type:
      <div id="choices">
          Breakfast
          <input type="radio" :id="breakfast" @change="changeHandler('Breakfast')" name="group1">
          Lunch
          <input type="radio" :id="lunch" @change="changeHandler('Lunch')" name="group1">
          Dinner
          <input type="radio" :id="dinner" @change="changeHandler('Dinner')" name="group1">
      </div>
  </div>
</template>

<script>
    import EventBus from "../../event-bus.js";

    export default {
        name: 'TypeSelector',
        props: ["errorReporter"],
        data() {
            return {
                breakfast : false,
                lunch : false,
                dinner : false
            }
        },
        mounted() {
            EventBus.$on("TypeSelector.validateType", e => {
                //console.log("Validating Type..." + e);
                if (e == null) {
                    console.log("");
                }
                if (this.breakfast == false && this.lunch == false && this.dinner == false) {
                    EventBus.$emit((this.errorReporter + "Error"), "Choose a meal type: Breakfast, Lunch, or Dinner");
                } else {
                    if (this.breakfast) {
                        EventBus.$emit("ValidationReturn", "Type,Breakfast");
                    }
                    if (this.lunch) {
                        EventBus.$emit("ValidationReturn", "Type,Lunch");
                    }
                    if (this.dinner) {
                        EventBus.$emit("ValidationReturn", "Type,Dinner");
                    }
                }
            });
        },
        methods: {
            changeHandler : function(arg) {
                //console.log("New meal type chosen: " + arg);
                if (arg == "Breakfast") {
                    this.breakfast = true;
                    this.lunch = false;
                    this.dinner = false;
                }
                if (arg == "Lunch") {
                    this.breakfast = false;
                    this.lunch = true;
                    this.dinner = false;
                }
                if (arg == "Dinner") {
                    this.breakfast = false;
                    this.lunch = false;
                    this.dinner = true;
                }
            }
        }
    }
</script>

<style>
    #choices {
        text-align: center;
        padding: 20px;
    }
</style>
