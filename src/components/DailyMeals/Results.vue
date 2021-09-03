<template>
  <div id="results">
      Totals
      <br>
      Calories: <b>{{ calories }}kcal</b>
      Protein: <b>{{ protein }}g</b>
      Carbs: <b>{{ carbs }}g</b>
      Fat: <b>{{ fat }}g</b>
  </div>
</template>

<script>
    import EventBus from "../../event-bus.js";

    export default {
        name: 'Results',
        props: ["errorReporter"],
        data() {
            return {
                calories : 0,
                protein : 0,
                carbs : 0,
                fat : 0
            }
        },
        mounted() {
            EventBus.$on("Results.contribute", e => {
                this.calories += parseInt(e.calories);
                this.protein += parseInt(e.protein);
                this.carbs += parseInt(e.carbs);
                this.fat += parseInt(e.fat);
            });
            EventBus.$on("Results.deContribute", e => {
                this.calories -= parseInt(e.calories);
                this.protein -= parseInt(e.protein);
                this.carbs -= parseInt(e.carbs);
                this.fat -= parseInt(e.fat);
            });
        }
    }
</script>

<style>
    #results {
        text-align: center;
        font-size: 30px;
        padding: 10px;
    }
</style>
