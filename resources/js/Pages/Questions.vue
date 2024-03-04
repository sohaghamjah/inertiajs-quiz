<script setup>
    import { Layout, NewQuestionModal } from '@/Shared';
import { router, usePage } from '@inertiajs/vue3';
    import { computed, ref } from 'vue';
    const showNewQuestionModal = ref(false);
    const createdNewQuestion = ref(null);
    const newAnswersField = ref([]);
    const selectedAnswer = ref(null);
    const page = usePage();
    const success = computed(() => page.props.flash.success);

    const closeQuestionModal = () => {
        showNewQuestionModal.value = false;
    }

    const openQuestionModal = () => {
        showNewQuestionModal.value = true;
    }

    let fieldCount = 1;

    const addNewAnswerField = () => {
        const newAnswerField = {
            id            : fieldCount++,
            answer        : '',
            correct_answer: 2
        }

        newAnswersField.value.push(newAnswerField);
    }

    const handleRadioToggleId = (id) => {
        selectedAnswer.value = id;
        newAnswersField.value.forEach(value => {
            if(value.id === id){
                value.correct_answer = 1;
            }else{
                value.correct_answer = 2;
            }
        });

    }

    const validatedAnswer = () => {
        for(const newField of newAnswersField.value){
            if(newField.answer.trim() == ''){
                return false;
            }
        }
        return true;
    }

    const answerFieldCount = () => {
        if(newAnswersField.length < 4){
            alert('Four Answer required to submit');
        }else if(newAnswersField.length  === 4){
            return true;
        }
        return false;
    }

    const submitQuestion = () => {
        if(!createdNewQuestion.value){
            alert('Question Cannot be empty');
            return false;
        }

        if(!validatedAnswer() && !answerFieldCount()){
            alert("Fill all inputs before submitting!");
            return false; 
        }

      
        router.post('/questions', {
            question:createdNewQuestion.value,
            answers: newAnswersField.value
        }); 
  
        router.on('success', () => {
            newAnswersField.value = [];
            createdNewQuestion.value = null;
            fieldCount = 1;
        })
    }

    const props = defineProps({
        questions: Object,
        errors: Object,
    });

</script>
<template>
    <Layout>
        <button class="btn btn-success" @click="openQuestionModal">Create</button>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Question</th>
                    <th scope="col">Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(question,index) in questions" :key="index">
                    <th scope="row">{{ index + 1 }}</th>
                    <td>{{ question.question }}</td>
                    <td>
                        <button class="btn btn-sm me-1 btn-success">View</button>
                        <button class="btn btn-sm me-1 btn-info">Edit</button>
                        <button class="btn btn-sm me-1 btn-danger">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>

        <Teleport to="body">
           <NewQuestionModal :show="showNewQuestionModal" @close="closeQuestionModal">
                <template #header>
                   <h3> Add New Question</h3>
                </template>
                <template #success>
                    <div class="alert alert-success" v-if="success">
                        {{ success }}
                    </div>
                </template>
                <template #body>
                    <form>
                        <div class="mb-3">
                            <label for="exampleInputEmail1" class="form-label">Email address</label>
                            <input type="email" class="form-control" v-model="createdNewQuestion" id="exampleInputEmail1" aria-describedby="emailHelp">
                        </div>
                        <table class="table">
                            <thead>
                                <tr>
                                    <th scope="row">#</th>
                                    <th>Answer</th>
                                    <th>Correct?</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(newField, index) in newAnswersField" :key="index">
                                    <th scope="row">{{ newField.id }}</th>
                                    <td>
                                        <input type="email" class="form-control" v-model="newField.answer" id="exampleInputEmail1" aria-describedby="emailHelp">
                                    </td>
                                    <td>
                                        <input :checked="newField.correct_answer === 1" :value="newField.id" @change="handleRadioToggleId(newField.id)" type="radio" class="form-check-input">
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </form>
                </template>
                <template #footer>
                    <button type="button" class="btn btn-primary me-2" v-if="newAnswersField.length < 4" @click="addNewAnswerField">+</button>
                    <button type="button" @click="closeQuestionModal" class="btn btn-danger me-2">Close</button>
                    <button type="submit" v-if="newAnswersField.length > 3" class="btn btn-success" @click="submitQuestion">submit</button>
                </template>
           </NewQuestionModal>
        </Teleport>

    </Layout>
</template>