<template>

    <div>
        <label for="plateNum">Plate Number:</label>
        <input type="text" id="plateNum" v-model="plateNum" />

        <label for="carSpecID">Car Spec ID:</label>
        <input type="text" id="carSpecID" v-model="carSpecID" />

        <button @click="addCar">Add Car</button>
    </div>

</template>
<script>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
    setup() {
        const plateNum = ref('');
        const carSpecID = ref('');
        const router = useRouter();


        const addCar = async () => {
            // Handle login logic here
            console.log('Plate Number:', plateNum.value);
            console.log('Car Spec:', carSpecID.value);


            const token = localStorage.getItem('jwt');
            if (!token) {
                alert('You need to be logged in to add items to the cart.');
                return;
            }

            axios.post('http://localhost:8000/add_new_car', {
                plate_num: plateNum.value,
                car_spec_id: parseInt(carSpecID.value)
            },
                {
                    headers: {
                        'Authorization': `Bearer ${token}`,
                        'Content-Type': 'application/json',
                    }
                }
            )
                .then(response => {
                    alert("Car added successfully");

                })
                .catch(error => {
                    console.error('Error adding car', error.response ? error.response.data : error);
                });



        };


        return {
            plateNum,
            carSpecID,
            addCar
        };
    }
};

</script>