<template>
  <div class="container py-5">
      <list-component :providers="computedProviders"/>
  </div>
</template>

<script>
import axios from "axios"

import moment from 'moment'
import ListComponent from './components/ListComponent'
export default{  
  data(){
    return {
        providers: []                
    }
  },
  components:{
    ListComponent 
  },
  mounted(){
    this.get()      
  }, 
  computed:{
    computedProviders(){
      return this.providers.map((provider) => {        
        if(provider.modified){
          provider.modified = moment(provider.modified).format("MMMM DD, YYYY")
        }
        return provider
      })
    }
  },
  methods:{
    async get(){
      const token = 'cJmAc71jah17sgqi1jqaksvaksda='
      try {
        const resp = await axios.get('https://pnny0h3cuf.execute-api.eu-west-1.amazonaws.com/dev/providers/1',{
                      headers: {
                        'Authorization': `Basic ${token}` 
                      }
                    })
        this.providers = resp.data        
      } catch (error) {
        console.log(error)
      }
    }    
      
  }
  
}
</script>

<style lang="scss">
    @import "~bootstrap/scss/bootstrap";
</style>
