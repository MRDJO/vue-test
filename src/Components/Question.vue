<template>
    <div class="question">
        <h3>{{ question.question }}</h3>

        <ul>
            <li v-for="(choice, index) in randomChoices"  :key="choice" >
                
                <Answer 
                    :value="choice" 
                    :id="`answer${index}`"   
                    @change="onAnswer"
                    v-model="answer"
                    :disabled="hasAnswer"
                    :correctAnswer="question.correct_answer"
                /> 
                <!-- <label :for="`answer${index}`" >
                    <input :disabled="hasAnswer"  v-model="answer"  :id="`answer${index}`"  :value="choice"  type="radio" name="answer"/>
                    {{ choice }}
                </label> -->
            </li>
        </ul>
        {{ answer }}
        <!-- <button :disabled="!hasAnswer"   @click="emits('answer', answer)" >Question suivante</button> -->
    </div>
</template>


<script setup>
    import { shuffleArray } from '@/functions/array';
    import {ref, computed, watch, onMounted, onUnmounted} from 'vue'
    import Answer from './Answer.vue';
     const props =  defineProps({
        question: Object
    })

    let timer 

    onMounted(() => {
        timer = setTimeout(()=>{
            answer.value =''
            onAnswer()
            // emits('answer', answer.value)
        }, 3000)  
    })

    onUnmounted(() => {
        clearTimeout(timer)
    })

    const onAnswer = () => {
        // answer.value = event.currentTarget.value
        clearTimeout(timer)
        timer = setTimeout(()=> {
            emits('answer', answer.value)
        }, 1000)
    }

    const answer = ref(null)
    const emits = defineEmits(['answer'])

    const hasAnswer = computed(()=> answer.value !== null )

    const randomChoices = computed(()=> shuffleArray(props.question.choices))
</script>


<style>

    .quetion{
        padding-top: 2rem;
    }
    .question button{
        margin-left: auto;
        display: block;
    }

</style>