<template>
  <div class="container mt-4">
    <div class="card">
      <div class="card-body">
        <div v-for="(item, index) in form" :key="index">
          <h3>Item {{ index + 1 }}</h3>
          <div class="row">
            <div class="col-sm-3">
              <label>Name</label>
              <input type="text" class="form-control" v-model="item.name">
            </div>
            <div class="col-sm-3">
              <label>Surname</label>
              <input type="text" class="form-control" v-model="item.surname">
            </div>
            <div class="col-sm-3">
              <label>Qualification</label>
              <input type="text" class="form-control" v-model="item.qualification">
            </div>
            <div class="col-sm-2">
              <button type="button" class="btn btn-danger btn-sm" @click="removeRow(index)">x</button>&nbsp;
              <button type="button" class="btn btn-success btn-sm" @click="addRow">+</button>
            </div>
          </div>
        </div>
        <button type="button" class="btn btn-success mt-3" @click="saveItem">Save</button>
      </div>
    </div>
  </div>
  
  <!-- List of previously entered forms -->
  <div class="container mt-4">
    <h2>List of Previously Entered Forms:</h2>
    <ul>
      <li v-for="(item, index) in savedForms" :key="index">
        <span>Name: {{ item.name }}, Surname: {{ item.surname }}, Qualification: {{ item.qualification }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'dynamic-form',
  setup() {
    const form = ref([
      { name: '', surname: '', qualification: '' }
    ]);

    const savedForms = ref([]);

    onMounted(() => {
      const savedData = localStorage.getItem('formDataList');
      if (savedData) {
        savedForms.value = JSON.parse(savedData);
      }
    });

    const addRow = () => {
      form.value.push({ name: '', surname: '', qualification: '' });
    }

    const removeRow = (index) => {
      if (form.value.length > 1) {
        form.value.splice(index, 1);
      }
    }

    const saveItem = () => {
      savedForms.value.push(...form.value);
      localStorage.setItem('formDataList', JSON.stringify(savedForms.value));
      form.value = [{ name: '', surname: '', qualification: '' }];
    }

    return {
      form,
      savedForms,
      addRow,
      removeRow,
      saveItem
    }
  }
}
</script>


<style>
.container {
  margin-top: 20px;
}

.card {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.card-body {
  display: flex;
  flex-direction: column;
}

.row {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.col-sm-3,
.col-sm-2 {
  flex: 1;
  margin-right: 10px;
}

.col-sm-2 {
  margin-right: 0;
  flex: 0.5;
}

.btn {
  margin-top: 10px;
}
</style>
