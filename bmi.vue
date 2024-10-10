<template>
  <div>
    <h2>BMI Calculator</h2>

    <div>
      <h2>Person 1</h2>
      <label for="height1">Height (in meters):</label>
      <input type="number" id="height1" step="0.01" v-model="person1.height">
      <label for="weight1">Weight (in kilograms):</label>
      <input type="number" id="weight1" v-model="person1.weight">
      <label for="name1">Name:</label>
      <input type="text" id="name1" v-model="person1.name">
    </div>

    <div>
      <h2>Person 2</h2>
      <label for="height2">Height (in meters):</label>
      <input type="number" id="height2" step="0.01" v-model="person2.height">
      <label for="weight2">Weight (in kilograms):</label>
      <input type="number" id="weight2" v-model="person2.weight">
      <label for="name2">Name:</label>
      <input type="text" id="name2" v-model="person2.name">
    </div>

    <button @click="computeBMI">Compute BMI</button>
    <button :disabled="!results.length || comparisonMade" @click="compareBMI">Compare BMI</button>

    <div v-if="results.length > 0">
      <h2>Results</h2>
      <div v-for="result in results" :key="result.name">
        <h3>{{ result.name }}</h3>
        <p>BMI: {{ result.bmi.toFixed(2) }}</p>
        <p>BMI Category: {{ result.bmiCategory }}</p>
        <p>Weight Classification: {{ result.weightClassification }}</p>
      </div>
      <div v-if="comparisonMade">
        <p v-if="comparisonResult">
          {{ comparisonResult.winner }} has a higher BMI than {{ comparisonResult.loser }}.
        </p>
        <p v-else>
          {{ results[0].name }} and {{ results[1].name }} have the same BMI.
        </p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      person1: {
        height: '',
        weight: '',
        name: ''
      },
      person2: {
        height: '',
        weight: '',
        name: ''
      },
      results: [],
      comparisonMade: false,
      comparisonResult: null
    };
  },
  methods: {
    computeBMI() {
      // Calculate BMI for each person
      const bmi1 = this.person1.weight / (this.person1.height * this.person1.height);
      const bmi2 = this.person2.weight / (this.person2.height * this.person2.height);

      // Determine BMI Category and Weight Classification for each person
      const bmiCategory1 = this.getBMICategory(bmi1);
      const bmiCategory2 = this.getBMICategory(bmi2);
      const weightClassification1 = this.getWeightClassification(bmi1);
      const weightClassification2 = this.getWeightClassification(bmi2);

      // Store results in an array
      this.results = [
        {
          name: this.person1.name,
          bmi: bmi1,
          bmiCategory: bmiCategory1,
          weightClassification: weightClassification1
        },
        {
          name: this.person2.name,
          bmi: bmi2,
          bmiCategory: bmiCategory2,
          weightClassification: weightClassification2
        }
      ];

      // Log the results array to the console for debugging (optional)
      console.log(this.results);

      // Reset the comparison flag
      this.comparisonMade = false;
    },
    compareBMI() {
        if (this.comparisonResult) {
        alert(`${this.comparisonResult.winner} has a higher BMI than ${this.comparisonResult.loser}.`);
      } else {
        alert(`${this.results[0].name} and ${this.results[1].name} have the same BMI.`);
      }
    },
    
    getBMICategory(bmi) {
      if (bmi < 18.5) {
        return "Underweight";
      } else if (bmi >= 18.5 && bmi < 25) {
        return "Normal weight";
      } else if (bmi >= 25 && bmi < 30) {
        return "Overweight";
      } else {
        return "Obese";
      }
    },
    getWeightClassification(bmi) {
      if (bmi < 18.5) {
        return "Underweight";
      } else if (bmi >= 18.5) {
        return "Healthy weight";
      } else {
        return "Class III Obesity";
      }
    }
  }
};
</script>