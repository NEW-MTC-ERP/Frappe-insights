<script setup>
import Checkbox from '@/components/Controls/Checkbox.vue'
import Color from '@/components/Controls/Color.vue'
import { FIELDTYPES } from '@/utils'
import { computed } from 'vue'

const emit = defineEmits(['update:modelValue'])
const props = defineProps({
	modelValue: { type: Object, required: true },
	columns: { type: Array, required: true },
})

const options = computed({
	get: () => props.modelValue,
	set: (value) => emit('update:modelValue', value),
})

const indexOptions = computed(() => {
	return props.columns
		?.filter((column) => !FIELDTYPES.NUMBER.includes(column.type))
		.map((column) => ({
			label: column.label,
			value: column.label,
			description: column.type,
		}))
})
const valueOptions = computed(() => {
	return props.columns
		?.filter((column) => FIELDTYPES.NUMBER.includes(column.type))
		.map((column) => ({
			label: column.label,
			value: column.label,
			description: column.type,
		}))
})
</script>

<template>
	<div class="space-y-4">
		<FormControl
			type="text"
			label="Title"
			class="w-full"
			v-model="options.title"
			placeholder="Title"
		/>
		<div>
			<label class="mb-1.5 block text-xs text-gray-600">Label Column</label>
			<Autocomplete v-model="options.xAxis" :returnValue="true" :options="indexOptions" />
		</div>
		<div>
			<label class="mb-1.5 block text-xs text-gray-600">Value Column</label>
			<Autocomplete v-model="options.yAxis" :returnValue="true" :options="valueOptions" />
		</div>

		<div>
			<label class="mb-1.5 block text-xs text-gray-600">Max Slices</label>
			<FormControl v-model="options.maxSlices" type="number" min="1" />
		</div>

		<Color
			label="Colors"
			v-model="options.colors"
			:max="parseInt(options.maxSlices) + 1"
			multiple
		/>

		<div v-show="!options.inlineLabels">
			<label class="mb-1.5 block text-xs text-gray-600">Label Position</label>
			<Autocomplete
				v-model="options.labelPosition"
				:options="['Top', 'Left', 'Bottom', 'Right']"
			/>
		</div>

		<Checkbox v-model="options.inlineLabels" label="Inline Labels" />
		<Checkbox v-model="options.scrollLabels" label="Paginate Labels" />
	</div>
</template>
