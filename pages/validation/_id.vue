<template>
    <div>

    </div>
</template>

<script>
    export default {
        mounted(){
            console.log(this.$route.params.id)
            this.userById()
        },
        methods:{
           async userById(){
                try {
                    const response = await this.$apollo.query({
                        query: require('~/gql/queries/users').GetUsersById,
                        variables:{
                            id:this.$route.params.id
                        }
                    })
                    console.log("User By Id: ", response.data)
                } catch (error) {
                    console.log("user by id: " ,error)
                }
            }
        }
    }
</script>

<style scoped>

</style>