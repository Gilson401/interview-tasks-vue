<script setup lang="ts">

import { ref, computed } from 'vue';
//I used the reactive property since they were missing 
const limit = ref<number>(100);
const hoveredNumber = ref<number | null>(null);

/**I transformed the number relation into a "computed". 
 * I used an array constructor to have more performance. 
 * The previous implementation uses a for-loop which has lower performance.*/
const numbers = computed<number[]>(() =>
	limit.value
		? Array.from({ length: limit.value }, (_, i) => i).sort(() => Math.random() - 0.5)
		: []
)

/**The "reset" and "hover" methods were removed as they were unnecessary since we can achieve the same effects through Vue's resources.
Furthermore, manipulating the DOM directly through "querySelectorAll" and similar methods is not recommended: 
it does not take advantage of Vue's reactivity and styling resources and can cause synchronization errors with reactive properties.*/

</script>
<template>
	<div>
		<span>
			Hovered Number: {{ hoveredNumber }}
		</span>
		<input type="number" v-model="limit" />
		<div class="container">

			<div class="number" :class="{ active: hoveredNumber && hoveredNumber % number === 0 }" :id="'number-' + numbers"
				v-for="number in numbers" :key="number" @mouseover="hoveredNumber = number"
				@mouseout="hoveredNumber = null">
				{{ number }}
			</div>
		</div>
	</div>
</template>

<style>

/* I made small improvements to the style in order to obtain a better user experience. */

input {
	display: block;
	margin-block: 20px;
}


.container {
	display: grid;
	grid-template-columns: repeat(10, 1fr);
}

.number {
	display: inline-block;
	padding: 5px;
	background-color: lightgrey;
	margin: 5px;
	text-align: center;
}

.active {
	background-color: red;
}
</style>