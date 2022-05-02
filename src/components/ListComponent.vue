<template lang="">

    <div class="card">
      <div class="card-header">
        <input type="text" placeholder="Name | Name Session" class="form-control" v-model="search">
      </div>
      <div class="card-body">
        <table-list :providers="providersFilter" />
      </div>
    </div>   

</template>
<script>
import TableList from './TableList'
export default {
    components:{
        TableList
    },
    props: ['providers'],
    data(){
        return{
            search: '',
            providersFilter: []
        }
    },
    watch:{
        search(){
            const search = this.search.toUpperCase()

            this.providersFilter = this.providers.filter(provider => {
                const session = provider.sessions[0].name.toUpperCase()
                const name = provider.name.toUpperCase()
                return session.indexOf(search) != -1 || name.indexOf(search) != -1
            })
        },
        providers(){
            this.providersFilter = this.providers
        }
    } 
    
}
</script>
