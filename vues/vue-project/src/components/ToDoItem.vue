<!-- <template>
  <li>
    <button @click="toggleTask">{{ model ? 'Odznacz' : 'Ukończ' }}</button>
    <span :style="{ 'text-decoration': model ? 'line-through' : 'none' }">{{ task }}</span>
  </li>
</template>

<script setup>
import { defineProps, defineModel } from 'vue';

const props = defineProps({
  task: {
    type: String,
    required: true
  },
  index:{
    type: Number,
    required: true
  }
});

const toggleTask = () => {
  model.value = !model.value;
};

const model = defineModel();
</script> -->
