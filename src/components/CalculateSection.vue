<script setup lang="ts">
import 'primeicons/primeicons.css'
import { ref } from 'vue'
import axios from 'axios' 
import HumanIcon from '../assets/human.svg'
import config from '../assets/config.json' 

const fromNode = ref<string>('')
const toNode = ref<string>('')
const resultPath = ref<string | null>(null)
const totalDistance = ref<number | null>(null)

const isLoading = ref<boolean>(false)

const calculatePath = async () => {
  if (fromNode.value && toNode.value) {
    isLoading.value = true; 

    try {
      const response = await axios.get(`${config.baseUrl}/api/ShortestPath`, { 
        params: {
          fromNode: fromNode.value,
          toNode: toNode.value
        },
        headers: {
          accept: 'text/plain'
        }
      });

   
      const data = response.data;

  
      const pathMatch = data.match(/fromNodeName = "(.*?)", toNodeName = "(.*?)": (.*)/);
      const distanceMatch = data.match(/Total Distance: (\d+)/);

      if (pathMatch && distanceMatch) {
        const path = pathMatch[3]; 
        const distance = parseInt(distanceMatch[1], 10); 

     
        resultPath.value = path;
        totalDistance.value = distance; 
      }
    } catch (error) {
      console.error('Error fetching the shortest path:', error);
    } finally {
      isLoading.value = false; 
    }
  }
};

const clearForm = () => {
  fromNode.value = '';
  toNode.value = '';
  resultPath.value = null;
  totalDistance.value = null;
  isLoading.value = false;
};
</script>


<template>
    <section class="bg-white w-[40vw] rounded-lg font-poppins overflow-hidden">
      <div class="grid grid-cols-2 gap-2">
        <div class="input-cols p-6">
          <div class="heading pb-5">
            <h1 class="text-primary text-2xl tracking-wide font-bold">
              Select Path
            </h1>
          </div>
          <!-- From node -->
          <div class="from-node">
            <label class="text-black" for="">From Node</label>
            <form class="max-w-sm mx-auto mt-2">
              <select
                v-model="fromNode"
                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full py-2 px"
              >
              <option value="" disabled selected>Select</option> 
                <option value="A">A</option>
                <option value="B">B</option>
                <option value="C">C</option>
                <option value="D">D</option>
                <option value="E">E</option>
                <option value="F">F</option>
                <option value="G">G</option>
                <option value="H">H</option>
                <option value="I">I</option>
              </select>
            </form>
          </div>
          <!-- To Node -->
          <div class="from-node mt-4">
            <label class="text-black" for="">To Node</label>
            <form class="max-w-sm mx-auto mt-2">
              <select
                v-model="toNode"
                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full py-2 px"
              >
              <option value="" disabled selected>Select</option> 
                <option value="A">A</option>
                <option value="B">B</option>
                <option value="C">C</option>
                <option value="D">D</option>
                <option value="E">E</option>
                <option value="F">F</option>
                <option value="G">G</option>
                <option value="H">H</option>
                <option value="I">I</option>
              </select>
            </form>
          </div>
          <div class="btn flex gap-2 mt-5">
            <div class="border border-btn py-2 px-3 rounded-lg text-btn cursor-pointer" @click="clearForm">
              Clear
            </div>
            <div class="flex border bg-btn py-2 px-3 rounded-lg text-btn gap-2 items-center cursor-pointer" @click="calculatePath">
              <span class="text-white">Calculate</span>
              <i class="pi pi-calculator" style="color: white"></i>
            </div>
          </div>
        </div>
        <div class="results-col">
          <div v-if="isLoading" class="flex justify-center items-center h-full">
            <i class="pi pi-spin pi-spinner" style="font-size: 2rem"></i>
          </div>
          <div v-else-if="resultPath && totalDistance !== null" class="results-visible p-6 bg-gray-200">
            <div class="heading pb-5">
              <h1 class="text-primary text-xl tracking-wide font-bold">
                Result
              </h1>
            </div>
            <div class="results p-2 rounded-lg text-md h-[40vh] bg-white">
              <div class="from">
                <span>From Node Name = "{{ fromNode }}", To Node Name = "{{ toNode }}": {{ resultPath }}</span>
              </div>
              <div class="total mt-2">
                <span>Total Distance: {{ totalDistance }}</span>
              </div>
            </div>
          </div>
          <div v-else class="human-icon p-6">
            <img :src="HumanIcon" alt="Human Icon" class="h-98 w-98" />
          </div>
        </div>
      </div>
    </section>
  </template>
  
