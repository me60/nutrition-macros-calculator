<template>
  <div id="new-meals-block">
      <div id="body">
          <TypeSelector :errorReporter="errorReporter" />
          <NameSelector :errorReporter="errorReporter" />
          <NutritionSelector :errorReporter="errorReporter" />
          <Submit :errorReporter="errorReporter" />
      </div>
      <ErrorFooter :setName="errorReporter" />
  </div>
</template>

<script>
    import ErrorFooter from '../Headers/ErrorFooter.vue';
    import TypeSelector from './TypeSelector.vue';
    import NameSelector from './NameSelector.vue';
    import NutritionSelector from './NutritionSelector.vue';
    import Submit from './Submit.vue';

    export default {
        name: 'NewMealsBlock',
        components: {
            ErrorFooter,
            TypeSelector,
            NameSelector,
            NutritionSelector,
            Submit
        },
        data() {
            return {
                errorReporter : "new_meal"
            }
        }
    }
</script>

<style>
    #body {
        background: gray;
        text-align: center;
        padding: 50px;
        font-style: italic, bold;
    }
</style>
