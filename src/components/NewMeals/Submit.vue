<template>
  <div id="submit">
      <FileHandler :errorReporter="errorReporter" />
      <div id="submit-button">
          <input type="submit" v-on:click="makeValidationChecks()" value="Click Here to Add Meal"/>
      </div>
  </div>
</template>

<script>
    import EventBus from "../../event-bus.js";
    import FileHandler from "../FileHandling/FileHandler.vue";

    export default {
        name: 'Submit',
        props: ["errorReporter"],
        data() {
            return {
                typeValidation : false,
                nameValidation : false,
                nutritionValidation : false,
                // we want the meal to not exist if it is being added
                mealDoesNotExistValidation : false,
                type : "",
                name : "",
                calories : 0,
                protein : 0,
                carbs : 0,
                fat : 0
            }
        },
        components : {
            FileHandler
        },
        mounted() {
            EventBus.$on("ValidationReturn", e => {
                this.addMealData(e);
                if (this.typeValidation && this.nameValidation && this.nutritionValidation && this.mealDoesNotExistValidation) {
                    this.typeValidation = false;
                    this.nameValidation = false;
                    this.nutritionValidation = false;
                    this.mealDoesNotExistValidation = false;
                    this.commitMeal();
                }
            });
        },
        methods : {
            getMealAsJSON : function() {
                let arr = [];
                arr.push({name:this.name, type:this.type, calories:this.calories, protein:this.protein, carbs:this.carbs, fat:this.fat});
                let jsonText = JSON.stringify(arr[0]);
                return jsonText;
            },
            addMealData : function(dataString) {
                dataString = dataString + "";
                let parts = dataString.split(',');
                if (parts[0] == "Type") {
                    this.typeValidation = true;
                    this.type = parts[1];
                } else if (parts[0] == "Name") {
                    this.nameValidation = true;
                    this.name = parts[1];
                } else if (parts[0] == "Nutrition") {
                    this.nutritionValidation = true;
                    this.calories = parts[1];
                    this.protein = parts[2];
                    this.carbs = parts[3];
                    this.fat = parts[4];
                } else if (parts[0] == "MealDoesNotExist") {
                    this.mealDoesNotExistValidation = true;
                } else {
                    console.log("Catastrophic Failure: Unsure of event emitted");
                }
            },
            commitMeal : function() {
                EventBus.$emit((this.errorReporter + "Success"), "Meal Committed");
                console.log("Submitting Meal!");
                console.log("New " + this.type + " item: " + this.name + ", calories: " + this.calories + ", protein: " + this.protein + ", carbs: " + this.carbs + ", fat: " + this.fat);
                EventBus.$emit("FileHandler.writeMeal", this.getMealAsJSON());
                EventBus.$emit("DailyMealsBlock.newMealCommitted", this.getMealAsJSON());
            },
            makeValidationChecks : function () {
                EventBus.$emit("TypeSelector.validateType", null);
                EventBus.$emit("NameSelector.validateName", null);
                EventBus.$emit("NutritionSelector.validateNutrition", null);
                EventBus.$emit("FileHandler.validateMealDoesNotExist", this.name);
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
