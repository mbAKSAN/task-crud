<template>
<v-container>

  <v-data-table
    :headers="headers"
    :items="desserts"
    :items-per-page="15"
    class="elevation-1" 
    id="datatable"
  ></v-data-table>

<v-form  @submit.prevent="PostUserInfo" v-model="valid">
    <v-container>
      <v-row>
        <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            v-model="firstName"
            name="firstName"
            :rules="firstName"
            :counter="10"
            label="First name"
            
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            v-model="lastName"
            :rules="lastName"
            :counter="10"
            label="Last name"
            
          ></v-text-field>
        </v-col>

  <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            v-model="city"
            :rules="city"
            label="City"
            
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            v-model="phoneNumber"
            :rules="phoneNumber"
            label="Phone Number"
            
          ></v-text-field>
        </v-col>
      </v-row>

    <div class="text-center">
    <v-btn
      dark
      color="red darken-2"
      @click=PostUserInfo    >
      Submit
    </v-btn>

    <v-snackbar
      v-model="snackbar"
      :multi-line="multiLine"
            :timeout="1500"

    >
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="red"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>
    </v-container>


  </v-form>

    </v-container>

</template>


<script>
export default {
  name: "HelloWorld",

  data: () => ({
     firstName: "",
    lastName: "",
    city: "",
    phoneNumber: "",

          headers: [
          {
            text: 'First Name:',
            align: 'start',
            sortable: false,
            value: 'firstName',    
          },
          { text: 'Last Name:', value: 'lastName' },
          { text: 'City:', value: 'city' },
          { text: 'Phone Number:', value: 'phoneNumber' },
         
        ], 
         desserts: [
        ],

        multiLine: true,
      snackbar: false,
      text: `Data Post`,

        
  }),
  
  methods: {
    GetUserInfo(){
    fetch("https://6057bb69c3f49200173acf30.mockapi.io/product")
    .then(async response => {
      const data = await response.json();
      console.log("Post İşlemi Başarılı");
      this.desserts = data;
      // check for error response
      if (!response.ok) {
        // GetUserInfo error message from body or default to response statusText
        const error = (data && data.message) || response.statusText;
        return Promise.reject(error);
      }

      this.totalVuePackages = data.total;
    })
    .catch(error => {
      this.errorMessage = error;
      console.error("There was an error!", error);
    });
  },  
      PostUserInfo(e) {

console.log(this.firstName)
  // PostUserInfo request using fetch with error handling
  const requestOptions = {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        firstName: this.firstName,
        lastName : this.lastName,
        city: this.city,
        phoneNumber: this.phoneNumber }),

  };
  fetch('https://6057bb69c3f49200173acf30.mockapi.io/product', requestOptions)
    .then(async response => {
      const data = await response.json();
      this.snackbar = true;
      // check for error response
      console.log("Post İşlemi Başarılı");
      console.log(data);
      this.GetUserInfo()
      
      if (!response.ok) {
        // GetUserInfo error message from body or default to response status
        const error = (data && data.message) || response.status;
        return Promise.reject(error);
      }

      this.PostUserInfoId = data.id;
    })
    .catch(error => {
      this.errorMessage = error;
      console.error('There was an error!', error);
    });
          e.preventDefault();

  },

},
 beforeMount(){
    this.GetUserInfo()
    alert("Data Get")
 },

};
</script>

<style scoped>
  
  td{
    text-align:center !important;

  }
</style>