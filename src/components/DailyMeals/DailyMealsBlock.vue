<template>
  <div id="daily-meals-block">
      <div id="body">
          <div id="breakfast">
              <div id="meal-tag"> Breakfast </div>
              <div v-for="breakfastItem in breakfastItems" :key="breakfastItem.name">
                  <fieldset id="group1">
                      <InteractableMealChoice :parentGroupName="'group1'" :name="breakfastItem.name" :calories="breakfastItem.calories" :protein="breakfastItem.protein" :carbs="breakfastItem.carbs" :fat="breakfastItem.fat" :errorReporter="errorReporter" />
                  </fieldset>
              </div>
          </div>
          <div id="lunch">
              <div id="meal-tag"> Lunch </div>
              <div v-for="lunchItem in lunchItems" :key="lunchItem.name">
                  <fieldset id="group2">
                      <InteractableMealChoice :parentGroupName="'group2'" :name="lunchItem.name" :calories="lunchItem.calories" :protein="lunchItem.protein" :carbs="lunchItem.carbs" :fat="lunchItem.fat" :errorReporter="errorReporter" />
                  </fieldset>
              </div>
          </div>
          <div id="dinner">
              <div id="meal-tag"> Dinner </div>
              <div v-for="dinnerItem in dinnerItems" :key="dinnerItem.name">
                  <fieldset id="group3">
                      <InteractableMealChoice :parentGroupName="'group3'" :name="dinnerItem.name" :calories="dinnerItem.calories" :protein="dinnerItem.protein" :carbs="dinnerItem.carbs" :fat="dinnerItem.fat" :errorReporter="errorReporter" />
                  </fieldset>
              </div>
          </div>
      </div>
      <Results :errorReporter="errorReporter" />
      <ErrorFooter :setName="errorReporter" />
  </div>
</template>

<script>
    import EventBus from "../../event-bus.js";
    import InteractableMealChoice from "./InteractableMealChoice.vue";
    import Results from "./Results.vue";
    import ErrorFooter from '../Headers/ErrorFooter.vue';

    export default {
        name: 'DailyMealsBlock',
        components: {
            ErrorFooter,
            InteractableMealChoice,
            Results
        },
        data() {
            return {
                componentKey : 0,
                breakfastItems : [],
                lunchItems : [],
                dinnerItems : [],
                errorReporter : "daily_meals"
            }
        },
        created() {
            this.populateChoices();
        },
        mounted() {
            EventBus.$on("DailyMealsBlock.mealDeleted", e => {
                console.log("removing " + e.name);
                let searchArr = [];
                if (e.parentGroupName == "group1") {
                    searchArr = this.breakfastItems;
                } else if (e.parentGroupName == "group2") {
                    searchArr = this.lunchItems;
                } else {
                    searchArr = this.dinnerItems;
                }
                for (let i = 0; i < searchArr.length; i++) {
                    if (searchArr[i].name == e.name) {
                        searchArr.splice(i, 1);
                        break;
                    }
                }
                EventBus.$emit((this.errorReporter + "Success"), (e.name + " deleted"));
                window.localStorage.removeItem(e.name);
            });
            EventBus.$on("DailyMealsBlock.newMealCommitted", e => {
                let mealObj = JSON.parse(e);
                let currentMeal = window.localStorage.getItem(mealObj.name);
                let mealObj2 = JSON.parse(currentMeal);
                if (mealObj2.type == "Breakfast") {
                    this.breakfastItems.push(mealObj2);
                } else if (mealObj2.type == "Lunch") {
                    this.lunchItems.push(mealObj2);
                } else {
                    this.dinnerItems.push(mealObj2);
                }
                EventBus.$emit((this.errorReporter + "Success"), (mealObj2.name + " added"));
            });
        },
        methods : {
            populateChoices : function() {
                for (let i = 0; i < window.localStorage.length; i++) {
                    let currentMeal = window.localStorage.getItem(window.localStorage.key(i));
                    if (currentMeal == "SILENT") {
                        continue;
                    }
                    let mealObj = JSON.parse(currentMeal);
                    if (mealObj.type == "Breakfast") {
                        this.breakfastItems.push(mealObj);
                    } else if (mealObj.type == "Lunch") {
                        this.lunchItems.push(mealObj);
                    } else {
                        this.dinnerItems.push(mealObj);
                    }
                }
            }
        }
    }
</script>

<style>
    #meal-tag {
        font-size: 30px;
        font-style: oblique;
    }
    #body {
        background: gray;
        text-align: center;
        padding: 50px;
        font-style: italic, bold;
    }
</style>
