<template>
    <label :for="id" :class="classes"  >
        <input :disabled="disabled"  v-model="model"  @change="onChange" :id="id"  :value="value"  type="radio" name="answer"/>
        {{ value }}
    </label>
</template>



<script setup>

    import {computed} from 'vue'

    const props = defineProps({
        id: String,
        disabled: Boolean,
        value: String,
        correctAnswer: String
    })

    const emits = defineEmits(['change'])

    const onChange = (event)=>{
        emits('change', event)
    }

    const model = defineModel()
    const classes = computed(()=>({
        right: props.disabled && props.value === props.correctAnswer,
        disabled: props.disabled,
        wrong: props.disabled && props.value !== props.correctAnswer && model.value === props.value
    }))


</script>


<style>

    .disabled{
        opacity: .5;
    }

    .right{
        color: green;
        opacity: 1;
    }

    .wrong{
        color: red;
        opacity: 1;
    }

</style>