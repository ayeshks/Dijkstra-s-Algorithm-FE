<script setup lang="ts">
import Calculate from '@/components/CalculateSection.vue'

interface HeadingProps {
  mainHeading: string;
  subHeading: string;
}


const props = withDefaults(defineProps<HeadingProps>(), {
  mainHeading: "Dijkstra’s Algorithm Calculator",
  subHeading: "Discovering Optimal Routes Through Nodes Using Dijkstra's Method"
});
</script>

<template>
    <section class="h-[45vh] bg-primary">
        <div class="primary-contain pb-32">
            <div class="heading-1 pt-12">
            <h1 class="text-white text-4xl font-poppines font-bold text-center">
                {{ props.mainHeading }}
            </h1>
        </div>
        <div class="heading-2 pt-5">
            <h1 class="text-white text-lg font-poppines font-medium text-center tracking-wide">
                {{ props.subHeading }}
            </h1>
        </div>
        </div>
        <div class="secound-contain flex justify-center items-center">
            <Calculate/>
        </div>       
    </section>
</template>
