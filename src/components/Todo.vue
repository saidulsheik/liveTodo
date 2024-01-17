<script setup>
import { ref, reactive } from 'vue';

const tasks = ref([
    { name: 'Task 1', time: 30 },
    { name: 'Task 2', time: 40 },
    { name: 'Task 3', time: 60 },
    { name: 'Task 4', time: 45 },
    { name: 'Task 5', time: 50 },
]);

const showPopup = ref(false);

const newTask = reactive({
    name: '',
    time: 0,
});

const removeTask = (index) => {
    tasks.value.splice(index, 1);
};

const openPopup = () => {
    showPopup.value = true;
};

const closePopup = () => {
    showPopup.value = false;
    newTask.name = '';
    newTask.time = 0;
};

const addTask = () => {
    if (newTask.name && newTask.time > 0) {
        tasks.value.push({ ...newTask });
        closePopup();
    } else {
        alert('Please enter valid task details.');
    }
};

const showModal = ref(false);
const editedTask = ref({ name: '', time: 0 });
const editedTaskIndex = ref(null);

const openEditModal = (index) => {
    editedTask.value = { ...tasks.value[index] };
    editedTaskIndex.value = index;
    showModal.value = true;
};

const updateTask = () => {
    if (editedTaskIndex.value !== null) {
        tasks.value[editedTaskIndex.value] = { ...editedTask.value };
        showModal.value = false;
    }
};

const closeModal = () => {
    showModal.value = false;
};

</script>


<template>
    <div id="app">
        <ul>
            <li v-for="(task, index) in tasks" :key="index">
                {{ `${task.name} - ${task.time} minutes` }}
                <button @click="openEditModal(index)">Edit</button>
            </li>
        </ul>

        <transition name="modal">
            <div v-if="showModal" class="modal-overlay">
                <div class="modal">
                    <form @submit.prevent="updateTask">
                        <label for="editName">Name:</label>
                        <input v-model="editedTask.name" type="text" id="editName" required>
                        <br>
                        <label for="editTime">Time (minutes):</label>
                        <input v-model="editedTask.time" type="number" id="editTime" required>
                        <br>
                        <button type="submit">Submit</button>
                    </form>
                    <button @click="closeModal">Close</button>
                </div>
            </div>
        </transition>
    </div>
</template>
<style></style>