<template>
  <form class="inputs-form" @submit.prevent="submitForm">
    <label for="name">Table Name</label>
    <input type="text" id="name" v-model="formData.tableName" >
    <p class="input-error">{{ errors['tableName'] }}</p>

    <label for="seat">Number of Seats</label>
    <input type="number" id="seat" v-model="formData.quantitySeat" >
    <p class="input-error">{{ errors['quantitySeat'] }}</p>

    <label for="area">Area</label>
    <select id="area" v-model="formData.area" >
      <option value="Indoor">Indoor</option>
      <option value="Outdoor">Outdoor</option>
      <option value="Terrace">Terrace</option>
      <option value="Counter">Counter</option>
    </select>
    <p class="input-error">{{ errors['area'] }}</p>

    <div class="reservable-section">
      <input type="checkbox" id="reservable" v-model="formData.reservable">
      <label for="reservable">Reservable</label>
    </div>
    
    
    <button type="submit">Add Table</button>
  </form>
</template>

<script setup>
import { reactive, ref, defineEmits, watch } from "vue";

const emits = defineEmits(['add-table']);

const formData = reactive({
  tableName: "",
  quantitySeat: null,
  area: "",
  reservable: false
});

const errors = ref({});

const formValidate = () => {
  errors.value = {}; // Reset the errors object on each submit

  // Validate Table Name
  if (!formData.tableName.trim()) {
    errors.value['tableName'] = 'Table Name is required.';
  } else if (Number(formData.tableName) && /^\d+$/.test(Number(formData.tableName))) {
    errors.value['tableName'] = `Table Name shouldn't contain only numbers.`;
  }


  // Validate Number of Seats
  if(formData.quantitySeat === null) {
    errors.value['quantitySeat'] = 'Number of Seats is required.';
  } else if (isNaN(formData.quantitySeat) || formData.quantitySeat <= 0) {
    errors.value['quantitySeat'] = 'Number of Seats must be a positive number.';
  }

  // Validate Area
  if (!formData.area) {
    errors.value['area'] = 'Please select an Area.';
  }

  // If there are no errors, submit the form
  if (Object.values(errors.value).length) {
    return false
  } else {
    return true
  }
}

const sendData = ref(false);

watch(
  () => formData,
  () => {
    if(sendData.value) {
      formValidate();
    }  
  },
  {
    deep: true
  }
)


const submitForm = () => {
  const checkValid = formValidate();
  sendData.value = true;
  if(checkValid) {
    emits('add-table', {...formData});
    clearFormData();
    sendData.value = false;
  } 
}

const clearFormData = () => {
  formData.tableName = "";
  formData.quantitySeat = null;
  formData.area = "";
  formData.reservable = false
}

</script>


<style lang="scss">
.inputs-form {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 5px;


  /* Style form labels */
  label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
    color: #333;
  }

  .reservable-section {
    display: flex;
    align-items: center;
    label {
      margin-bottom: 0;
    }
    label:hover {
      color: #3A864A;
    }

        /* Style the checkboxes */
    input[type="checkbox"] {
      appearance: none;
      width: 18px;
      height: 18px;
      border: 2px solid #5ABF6E;
      border-radius: 3px;
      vertical-align: middle;
      position: relative;
      cursor: pointer;
    }

    input[type="checkbox"]:checked::before {
      content: "";
      position: absolute;
      top: 2px;
      left: 2px;
      width: 14px;
      height: 14px;
      background-color: #5ABF6E;
      border-radius: 2px;
    }
  }

  /* Style form inputs and select */
  input[type="text"],
  input[type="number"] {
    width: calc(100% - 20px);
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px; 
  }
  select {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }

  /* Style the checkbox and label */
  input[type="checkbox"] {
    margin-right: 5px;
    vertical-align: middle;
  }

  /* Style form submit button */
  button[type="submit"] {
    display: block;
    margin: 10px auto;
    padding: 10px 15px;
    background-color: #5ABF6E;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  button[type="submit"]:hover {
    background-color: #4DA35E;
  }

  .input-error {
    margin: 5px 0 15px 0;
    height: 16px;
    font-size: 14px;
    color: red;
  }
}
</style>
