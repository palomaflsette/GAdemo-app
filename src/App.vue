<template>
  <div>
    <p><input v-model="num_experiments" type="number" placeholder="Enter number of experiments" /></p>
    <p><input v-model="func_str" type="string" placeholder="Enter the function here" /></p>
    <p><input v-model="exec_chars.num_generations" type="number" placeholder="Number of generations" /></p>
    <p><input v-model="exec_chars.population_size" type="number" placeholder="Population size" /></p>
    <p><input v-model="exec_chars.crossover_rate" type="number" step="0.01" placeholder="Crossover rate" /></p>
    <p><input v-model="exec_chars.mutation_rate" type="number" step="0.01" placeholder="Mutation rate" /></p>
    <p><input v-model="exec_chars.maximize" type="checkbox" /> Maximize</p>
    <p><input v-model="exec_chars.interval[0]" type="number" placeholder="Interval start" /></p>
    <p><input v-model="exec_chars.interval[1]" type="number" placeholder="Interval end" /></p>
    <p>
      <label>Crossover type:</label>
      <input v-model="exec_chars.crossover_type.one_point" type="checkbox" /> One Point
      <input v-model="exec_chars.crossover_type.two_point" type="checkbox" /> Two Point
      <input v-model="exec_chars.crossover_type.uniform" type="checkbox" /> Uniform
    </p>
    <p><input v-model="exec_chars.normalize_linear" type="checkbox" /> Normalize Linear</p>
    <p><input v-model="exec_chars.normalize_min" type="number" placeholder="Normalize min" /></p>
    <p><input v-model="exec_chars.normalize_max" type="number" placeholder="Normalize max" /></p>
    <p><input v-model="exec_chars.elitism" type="checkbox" /> Elitism</p>
    <p><input v-model="exec_chars.steady_state" type="checkbox" /> Steady State</p>
    <p><input v-model="exec_chars.steady_state_without_duplicateds" type="checkbox" /> Steady State without Duplicateds</p>
    <p><input v-model="exec_chars.gap" type="number" step="0.01" placeholder="Gap" /></p>
    <p><button @click="runExperiments">Run Experiments</button></p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      num_experiments: 1,
      func_str: '0.5 - (sin(sqrt(x**2 + y**2))**2 - 0.5) / (1 + 0.001 * (x**2 + y**2))**2',
      exec_chars: {
        num_generations: 1,
        population_size: 20,
        crossover_rate: 0.65,
        mutation_rate: 0.8,
        maximize: true,
        interval: [-2, 2],
        crossover_type: {
          one_point: true,
          two_point: false,
          uniform: false
        },
        normalize_linear: false,
        normalize_min: 0,
        normalize_max: 100,
        elitism: false,
        steady_state: false,
        steady_state_without_duplicateds: true,
        gap: 80
      }
    };
  },
  methods: {
    async runExperiments() {
      try {
        console.log('Sending num_experiments:', this.num_experiments);
        console.log('Sending exec_chars:', this.exec_chars);
        console.log('Sending func_str:', this.func_str);

        const response = await axios.post(
          `http://127.0.0.1:8000/run-experiments?func_str=${encodeURIComponent(this.func_str)}&num_experiments=${this.num_experiments}`, 
          this.exec_chars
        );
        console.log('Response:', response.data);
      } catch (error) {
        console.error('Error:', error.response ? error.response.data : error.message);
      }
    }
  }
};
</script>

<style scoped>
/* Your styles here */
</style>
