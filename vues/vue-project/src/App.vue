<!-- <template>
  <div>
    <h1>{{ name }}</h1>
    <input type="text" :value="name" @input="handleInput">
    <input type="text" :value="imie" @input="handleInputimie">
    <input type="text" :value="nazwisko" @input="handleInputnazw">
    <br><br>
    <p>Moje imię: {{ imie }}, moje nazwisko: {{ nazwisko }}</p>
    <p>Imię i nazwisko zrobione fullnamem: {{ fullname }}</p>
    <br><br>
    <select v-model="selectedColor">
      <option value="red">Czerwony</option>
      <option value="green">Zielony</option>
      <option value="blue">Niebieski</option>
    </select>
    <br><br>
    <p>Wybrany kolor: <span :style="{ color: selectedColor }">{{ selectedColor }}</span></p>
    <ToDoInput @create="handleCreateTask" />
    <ul>
      <template v-for="(task, index) in todo" :key="index">  
    <ToDoItem v-bind:task="task.text" v-bind:index="index" v-model="task.finished" v-on:changed="toggleTask" />
      </template>
    </ul>
    <div v-if="warned">
  <p>Rob zadanka a nie...</p>
  <button @click="hideWarning">OK</button>
</div>
    <p>{{ progressText }}</p>
    <p>Ukończono {{ finishedCount }} z {{ todo.length }} zadań</p>
  </div>
</template>

<script setup>
import { ref, reactive, computed, watch, defineEmits, defineProps } from 'vue';
import ToDoInput from './components/ToDoInput.vue';
import ToDoItem from './components/ToDoItem.vue';
const updateTask = (updatedTask) => {
  const index = todo.value.findIndex(task => task === updatedTask); 
  if (index !== -1) {
    todo.value.splice(index, 1, updatedTask);
  }
};
const name = ref('Julcia');
const imie = ref('JAJAJA');
const nazwisko = ref('Kowal');
const user = reactive({
  firstname: 'Imię',
  lastname: 'Nazwisko',
});

const handleInput = (event) => {
  name.value = event.target.value;
};
const handleInputimie = (event) => {
  imie.value = event.target.value;
};
const handleInputnazw = (event) => {
  nazwisko.value = event.target.value;
};

const todo = ref([
  {
    text: "Zrobić matmę",
    finished: false
  },
  {
    text: "Zrobić polski",
    finished: false
  },
  {
    text: "Zrobić angielski",
    finished: false
  },
  {
    text: "Zrobić niemiecki",
    finished: false
  },
  {
    text: "Zrobić wf",
    finished: false
  },
  {
    text: "Wyplakac sie w poduszke",
    finished: false
  },
  {
    text: "poglaskac kotki na ktore mam alergie",
    finished: false
  },
  {
    text: "oddychac",
    finished: true
  }
]);

const handleCreateTask = (task) => {
  if (task.trim() !== '') {
    todo.value.push({ text: task, finished: false });
  }
};



const fullname = computed(() => imie.value + ' ' + nazwisko.value);
const cols = ['red', 'blue', 'green'];
const selectedColor = ref(cols[0]);
const warned = ref(false);

const finished = computed(() => todo.value.filter(task => !task.finished).length);
watch(todo, (newTodo) => {
  const unfinishedTasks = newTodo.filter(task => !task.finished);
  if (unfinishedTasks.length > 5) {
    warned.value = true;
  } else {
    warned.value = false;
  }
}, {deep: true});

const finishedCount = computed(() => todo.value.filter(task => task.finished).length);

const progressText = computed(() => {
  if (finished.value === 0) {
    return "Fajrant!";
  } else if (finished.value > 0 && finished.value < todo.value.length - 3) {
    return "Robota wre!";
  } else if (finished.value >= todo.value.length - 3 && finished.value < todo.value.length) {
    return "Czas się wziąć do roboty";
  } else {
    return "Już prawie koniec...";
  }
});

const hideWarning = () => {
  warned.value = false;
};
</script> -->

<template>
  <div>
    <input type="text" v-model="search" placeholder="Search users">
    <button @click="searchUsers">Search</button>
    <GithubCardWrapper v-if="displayedUsers.length > 0" :usersData="displayedUsers" />
    <p v-if="searched && displayedUsers.length === 0">
      Nie znaleziono użytkownika o nazwie "{{ search }}".
    </p>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import GithubCardWrapper from './components/GithubCardWrapper.vue';
const search = ref('');
const displayedUsers = ref([]);
const searched = ref(false);
const responseUsers = ref([]);
const searchUsers = async () => {
  searched.value = true;
  try {
    const response = await fetch(`https://api.github.com/search/users?q=${search.value}`);
    if (response.ok) {
      const data = await response.json();
      responseUsers.value = data.items;
      await Promise.all(responseUsers.value.map(async (user) => {
        try {
          const userDetailsResponse = await fetch(`https://api.github.com/users/${user.login}`);
          if (userDetailsResponse.ok) {
            const userDetails = await userDetailsResponse.json();
            user.details = userDetails;
          } else {
            throw new Error('Failed to fetch user details');
          }
        } catch (error) {
          console.error('Error fetching user details:', error);
        }
      }));
      displayedUsers.value = responseUsers.value;
    } else {
      throw new Error('Failed to fetch users data');
    }
  } catch (error) {
    console.error('Error fetching users data:', error);
  }
};
</script>



