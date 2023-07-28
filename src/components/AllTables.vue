<template>
    <div class="show-tables">
        <div class="filter-section">
            <h1>Filter Tables</h1> 
            <input type="checkbox" id="show-all" v-model="showAll">
            <label for="show-all">All</label>
            <input type="checkbox" id="show-reservable" v-model="showReservables">
            <label for="show-reservable">Reservable</label>
        </div>
        <div class="table-container">
          <table>
            <thead>
                <tr>
                    <th>Table Name</th>
                    <th>Number of Seats</th>
                    <th>Area</th>
                    <th>Reservable</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody border="1">
                <tr v-for="(data, index) in showingTablesData" :key="index">
                    <td>{{ data.tableName }}</td>
                    <td>{{ data.quantitySeat }}</td>
                    <td>{{ data.area }}</td>
                    <td>{{ showingStatusReservable(data.reservable) }}</td>
                    <td>
                        <button class="delete-table" @click="removeTable(index)"> X </button>
                    </td>
                </tr>
            </tbody>
        </table>  
        </div>
        
    </div>
</template>

<script setup>
import { defineProps, defineEmits, computed, ref, watch } from 'vue'

const emits = defineEmits(['remove-table']);

const props = defineProps({
    tables: {
        type: Array
    }
})

// datas for filtering
const showAll = ref(true);
const showReservables = ref(false);

// I wrote the following code (2 watches) because when user checks the Reservables option, the all option should be removed, 
// but I wrote the if statement because when the user unchecks the Reservables option, 
// it shouldn't automatically select the all option.

watch(
    () => showReservables.value,
    (reservables) => {
        if(reservables) {
            showAll.value = false;
        }
    }
)

watch(
    () => showAll.value,
    (all) => {
        if(all) {
            showReservables.value = false;
        }
    }
)

const showingTablesData = computed(() => {
    if(showAll.value) {
        return props.tables
    } else if(showReservables.value) {
        return props.tables.filter(table => table.reservable)
    }
    return []
})

const showingStatusReservable = (reservable) => reservable ? 'Yes' : 'No';

const removeTable = (index) => {
    emits('remove-table', index);
}
</script>

<style lang="scss">

.show-tables {
    .filter-section {
        max-width: 400px;
        margin: 40px auto;
        padding: 20px;
        background-color: #f9f9f9;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        border-radius: 10px;

        /* Style the heading */
        h1 {
          margin-bottom: 20px;
          text-align: center;
          font-size: 28px;
          font-weight: bold;
          color: #333;
          text-transform: uppercase;
        }

        /* Style the checkbox and label */
        input[type="checkbox"] {
          margin-right: 10px;
          vertical-align: middle;
        }

        label {
          font-size: 18px;
          color: #444;
          cursor: pointer;
          margin-right: 25px;
        }

        /* Adjust spacing between checkboxes and labels */
        input[type="checkbox"] + label {
          margin-left: 15px;
        }

        /* Hover effect for labels */
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
    /* Style the table */
    .table-container {
        width: 100%;
        overflow-x: auto;
       table {
            overflow: hidden;
            width: 100%;
            border-collapse: collapse;    
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            background-color: #fff;  
            /* Style the table header */
            thead {
                background-color: #f2f2f2;
                th {
                    text-align: center;
                    padding: 10px;
                    font-weight: bold;
                }
            }

            /* Style the table body */
            tbody {
                font-size: 14px;
                /* Style alternating table rows */
                tr:nth-child(even) {
                    background-color: #f9f9f9;
                }

                /* Style table cells */
                td {
                    padding: 10px;
                    text-align: center;            
                    /* Style the delete button */
                    .delete-table {
                    padding: 5px 10px;
                    background-color: #e74c3c;
                    color: #fff;
                    border: none;
                    border-radius: 3px;
                    cursor: pointer;
                    }

                    .delete-table:hover {
                    background-color: #c0392b;
                    }
                }
            }
        }  
    }
     
}
</style>