<template>
  <div class="container">
    <div class="background-image">
    <div class="card">
    <form @submit.prevent="submitForm">
      <label for="name">Your Name:</label>
      <input type="text" id="name" v-model="name" pattern="[A-Za-z ]*" @keyup="filterNonCharacterInput" required />

      <label for="language">What is your favorite language?</label>
      <select id="language" v-model="favoriteLanguage">
        <option v-for="language in languages" :value="language" :key="language">{{ language }}</option>
      </select>

      <button type="submit">Submit</button>
    </form>
  </div>
</div>
</div>
</template>

<script>
import axios from 'axios';
export default {

  data() {
    return {
      name: '',
      favoriteLanguage: '',
      languages:[]
    };
  },
  created() {
    this.getLanguage();
  },
  methods: {
    filterNonCharacterInput() {
      this.name = this.name.replace(/[^a-zA-Z]/g, '');
    },
    getLanguage(){
      axios.get('https://www.cc.com/languages/endpoint',{
        headers: {
          //'Content-Type': 'application/json',
          'Accept': 'application/json',
         
        }
      })
        .then(res => {
          console.log("sneha",res);
          this.languages = res.data;
        })
        .catch(error => {
          console.error('Error fetching languages:', error);
      });
    },
    submitForm(event) {
      event.preventDefault();
      console.log('Name:', this.name);
      const payload = {
        name: this.name,
        language: this.language,
      };
      axios.post('https://www.cc.com/languages/endpoint', payload)
      .then(response => {
        console.log(response.data);
      })
      .catch(error => {
        console.error(error);
      });
      // Reset the form fields
      this.name = '';
      this.language = '';
    },
  },
};
</script>
<style>

.container {
  /* display: flex; */
  position: relative;
  /* justify-content: center; */
  /* align-items: center; */
  /* height: 100vh; */
  /* background-image: url(~@/assets/4479865.jpg); */
  /* background-size: cover; */
}

.card{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  /* Other card styling properties */
}


.background-image {
  position: relative;
  background-image: url(~@/assets/4479865.jpg);
  background-size: cover;
  background-position: center;
  height: 645px; /* Adjust the height as needed */
  border-radius: 8px;
}

form {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-bottom: 20px;
}

label {
  margin-bottom: 5px;
}

input[type="text"],
select {
  padding: 5px;
  border-radius: 4px;
  border: 1px solid #ccc;
  margin-bottom: 10px;
  width: 200px;
}

button[type="submit"] {
  padding: 8px 15px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button[type="submit"]:hover {
  background-color: #45a049;
}

input:invalid {
  border-color: red;
}

input:invalid + span::before {
  content: '✖ ';
  color: red;
}

input:valid + span::before {
  content: '✓ ';
  color: green;
}
</style>
