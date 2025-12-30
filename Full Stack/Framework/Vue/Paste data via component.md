
```vue
<script setup>
const props = defineProps<{
	moduleValue: string;
}>()

const emit = defineEmits(["update:moduleValue"])

const writeableComputed({
	get(){
		return props.moduleValue;
	},
	set(value){
		emit("update:moduleValue", value);
	},
});
</script>

<template>
	<input v-model="writeableComputed" /> 
</template>
```