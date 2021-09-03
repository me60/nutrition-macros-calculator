<template>
  <div id="interactable-meal-choice">
      <div id="content-zone">
          <button v-on:click="deleteMeal()"> x </button>
          {{ name }} - Calories(kcal): {{ calories }} - Protein(g): {{ protein }} - Carbs(g): {{ carbs }} - Fat(g): {{ fat }}
          <input type="radio" v-on:change="activityChange()" :checked="activated"/>
      </div>
  </div>
</template>

<script>
    import EventBus from "../../event-bus.js";

    export default {
        name: 'InteractableMealChoice',
        props: ["parentGroupName","name","calories","protein","carbs","fat","errorReporter"],
        data () {
            return {
                activated : false
            }
        },
        mounted() {
            EventBus.$on("InteractableMealChoice.setFalse", e => {
                if (e == this.parentGroupName) {
                    if (this.activated) {
                        EventBus.$emit("Results.deContribute", this);
                    }
                    this.activated = false;
                }
            });
        },
        methods : {
            activityChange : function() {
                EventBus.$emit("InteractableMealChoice.setFalse", this.parentGroupName);
                this.activated = true;
                EventBus.$emit("Results.contribute", this);
            },
            deleteMeal : function() {
                EventBus.$emit("DailyMealsBlock.mealDeleted", this);
            }
        }
    }
</script>

<style>
    #interactable-meal-choice {
        text-align: center;
        padding: 10px;
    }
</style>
