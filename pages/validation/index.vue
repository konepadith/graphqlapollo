<template>
  <div>
    <v-form ref="myform">
      <v-text-field
        v-model="name"
        :rules="rules"
        label="Name"
        type="text"
        required
        :counter="10"
      ></v-text-field>
      <v-text-field
        v-model="age"
        :rules="rules"
        label="Age"
        type="number"
      ></v-text-field>
      <v-text-field
        v-model="position"
        :rules="rules"
        label="Position"
        type="text"
      ></v-text-field>
      <v-btn
        type="button"
        block
        class="mt-2"
        v-if="!action"
        @click="insertUser"
      >
        Submit</v-btn
      >
      <v-btn type="button" rounded text v-if="action" @click="updateCharacter()" >Update</v-btn>
    </v-form>
    <div class="d-flex flex-row flex-wrap">
      <v-card
        class="ma-2"
        max-width="344"
        outlined
        v-for="item in character"
        :key="item.id"
      >
        <v-list-item three-line>
          <v-list-item-content>
            <div class="text-overline mb-4">
              {{ item.name }}
            </div>
            <v-list-item-title class="text-h5 mb-1">
              {{ item.age }} || {{ item.id }}
            </v-list-item-title>
            <v-list-item-subtitle>{{ item.position }}</v-list-item-subtitle>
          </v-list-item-content>
          <nuxt-link :to="{name:'validation-id',params:{id:item.id} }">
            <v-list-item-avatar tile size="80" color="grey" ></v-list-item-avatar>
          </nuxt-link>
          
        </v-list-item>

        <v-card-actions>
          <v-btn outlined rounded text @click="deleteUser(item.id)"> Delete </v-btn>
          <v-btn
            rounded
            text
            @click="
              buttonAction({
                id: item.id,
                name: item.name,
                age: item.age,
                position: item.position,
              })
            "
            >Update</v-btn
          >
        </v-card-actions>
      </v-card>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      character: [],
      action: false,
      name: "",
      age: null,
      position: "",
      updatedata: {},
      rules: [
        (value) => {
          if (value) return true;

          return "You must enter a first name";
        },
      ],
    };
  },
  mounted(){
    this.showUser()
  },
  methods: {
    async insertUser(event) {
      if (this.$refs.myform.validate() == false) {
        alert("Validation is false");
        // console.log(this.$refs.myform.validate())
      } else {
        try {
          const response = await this.$apollo.mutate({
          mutation: require("~/gql/mutations/users").insertUser,
          variables: {
            name: this.name,
            age: this.age,
            position: this.position,
          },
        })
        this.showUser()
        console.log(this.$refs.myform.validate());
        this.$refs.myform.reset();
        } catch (error) {
          
        }
      }
    },
    buttonAction(data) {
      this.action = true;
      this.id = data.id;
      this.name = data.name;
      this.age = data.age;
      this.position = data.position;
    },
    updateCharacter() {
      if (this.$refs.myform.validate() == false) {
        alert("Validation is false");
        console.log(this.$refs.myform.validate())
      } else {
        this.updatedata = {
          id: this.id,
          name: this.name,
          age: this.age,
          position: this.position,
        };
        this.updateUser();
        this.action = false;
        console.log(this.$refs.myform.validate());
        this.$refs.myform.reset();
      }
    },
    async showUser() {
    try {
        const response = await this.$apollo.query({
          
          query: require("~/gql/queries/users").userQueries,
          fetchPolicy: 'no-cache',
        })
        console.log("showUser",response.data.users)
        this.character = response.data.users;
    } catch (error) {
        console.log("Error is", error);
    }
    },

   async updateUser() {
      try {
        const response = await this.$apollo
        .mutate({
          mutation: require("~/gql/mutations/users").userUpdate,
          variables: {
            id: this.updatedata.id,
            name: this.updatedata.name,
            age: this.updatedata.age,
            position: this.updatedata.position,
          },
        })
        this.showUser()
      } catch (error) {
        console.log("updateUser:",error)
      }
    
    },
    insertUsergql() {
      this.$apollo.mutate({
          mutation: require("~/gql/mutations/users").insertUser,
          variables: {
            name: this.name,
            age: this.age,
            position: this.position,
          }
        })
        .then((response) => {
          console.log("insertUser", response);
        })
        .catch((error) => {
          console.log("Error insert", error);
        });
        
    },
    async deleteUser(id){
        try {
          const response = await this.$apollo.mutate({
            mutation:require('~/gql/mutations/users').deleteUser,
            variables:{
                id:id,
            }
        })
        console.log(response)
        this.showUser()
        } catch (error) {
          console.log("Delete:",error)
        }
    }
  },
};
</script>

<style scoped>
</style>