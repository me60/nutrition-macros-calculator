<template>
  <div id="file-handler">
  </div>
</template>

<script>
    import EventBus from "../../event-bus.js";

    export default {
        name: 'FileHandler',
        props: ["errorReporter"],
        data() {
            return {
                filepath : "../../meals.json"
            }
        },
        mounted() {
            EventBus.$on("FileHandler.validateMealDoesNotExist", e => {
                // return a ValidationReturn if the meal does not exist
                // e = meal name
                if (this.mealExists(e)) {
                    EventBus.$emit((this.errorReporter + "Error"), "Meal exists already");
                } else {
                    // payload is arbitrary, signal is implicit of success
                    EventBus.$emit("ValidationReturn", "MealDoesNotExist,");
                }
            });
            EventBus.$on("FileHandler.writeMeal", e => {
                this.writeMeal(e);
            });
        },
        methods : {
            writeMeal : function(mealData) {
                try {
                    console.log("Writing " + mealData + " to storage.");
                    let mealObj = JSON.parse(mealData);
                    window.localStorage.setItem(mealObj.name, mealData);
                } catch(e) {
                    console.log("Catastrophic Failure: Couldn't write to storage! Trace: " + e);
                }
            },
            mealExists : function(mealName) {
                try {
                    let storeReturn = window.localStorage.getItem(mealName);
                    console.log("Return from the store: " + storeReturn + " from entry " + mealName);
                    if (storeReturn == null || mealName == "") {
                        return false;
                    } else {
                        return true;
                    }
                } catch(e) {
                    console.log("Catastrophic Failure: Couldn't read from storage! Trace: " + e);
                }
            }
        }
    }
</script>

<style>
</style>
