<template>
<div>
  <h1>Hello</h1>
</div>
 
</template>

<script>
export default {
  data() {
    return {
      characters: []
    };
  },
  mounted() {
    this.ShowUser();
  },
  methods: {
    ShowUser() {
      this.$apollo.query({
          query: require('~/gql/queries/users').userQueries
        })
        .then((response) => {
          console.log(response.data.users)
          this.characters = response.data.users
        })
        .catch((error) => {
          console.log("Error fetching characters", error);
        });
    },
    UpdateUser() {
      
       this.$apollo.mutate({
          mutation:require ('~/gql/mutations/users').userUpdate,
          variables:{
            id:2,
            name:"khui",
            age:14,
            position:"God"
          }
        }).then((response)=>{
          console.log(response)
        })
        .catch((error)=>{
          console.log("Error updating",error)
        })
      
    },
   async insertUser(){
      try {
        const response = await this.$apollo.mutate({
          variables:{
            name:"peter",
          age:15,
          position:"Gamer"
        }
        })
        console.log(response)
      } catch (error) {
        console.log("Inser",error)
      }
    },
    deleteUser(){
      this.$apollo.mutate({
        mutation:require('~/gql/mutations/users').deleteUser,
        variables:{
          id:3
        }
      }).then((response)=>{
        console.log(response)
      }).catch((error)=>{
        console.log("Error",error)
      })
    }
  },
};
</script>
