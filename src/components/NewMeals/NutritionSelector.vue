<template>
  <div id="nutrition-selector">
      Nutrition:
      <div id="choices">
          Calories (kcal):
          <input type="text" v-model="calories" @change="changeHandler('calories')">
          Protein (g):
          <input type="text" v-model="protein" @change="changeHandler('protein')">
          Carbs (g):
          <input type="text" v-model="carbs" @change="changeHandler('carbs')">
          Fat (g):
          <input type="text" v-model="fat" @change="changeHandler('fat')">
      </div>
  </div>
</template>

<script>
    import EventBus from "../../event-bus.js";

    export default {
        name: 'NutritionSelector',
        props: ["errorReporter"],
        data() {
            return {
                calories : "",
                protein : "",
                carbs : "",
                fat : ""
            }
        },
        mounted() {
            EventBus.$on("NutritionSelector.validateNutrition", e => {
                //console.log("Validating Nutrition..." + e);
                // matches positive integers and 0
                if (e == null) {
                    console.log("");
                }
                let regex = /^[0-9]\d*$/;
                let calCheck = this.calories.match(regex);
                let proCheck = this.protein.match(regex);
                let carCheck = this.carbs.match(regex);
                let fatCheck = this.fat.match(regex);
                if (calCheck == null || proCheck == null || carCheck == null || fatCheck == null) {
                    EventBus.$emit((this.errorReporter + "Error"), "Enter valid nutrition information");
                } else {
                    EventBus.$emit("ValidationReturn", "Nutrition," + this.calories + "," + this.protein + "," + this.carbs + "," + this.fat);
                }
            });
        },
        methods : {
            changeHandler : function () {
                //console.log("New " + arg);
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
