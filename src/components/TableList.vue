<template >
    
        <table class="table table-striped">
            <thead>
                <tr>
                    <td @click="sort('id')">ID</td>
                    <td @click="sort('name')">Name</td>
                    <td @click="sort('access_group_name')">Group Name</td>
                    <td @click="sort('modified')">Modified</td>
                    <td @click="sort('sessions_name')">Name Session</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(provider, index) in arrItem"  v-show="index >= minItem && index < maxItem" :key="provider.id">
                    <th>{{provider.id}}</th>
                    <th>{{provider.name}}</th>
                    <th>{{provider.access_group_name}}</th>
                    <th>{{provider.modified}}</th>
                    <th>{{provider.sessions[0].name}}</th>
                </tr>
            </tbody>
        </table>
        <paginate-item :totalPage="totalPage" :currentPage="currentPage" :maxVisible="5" @changePage="changePage" />
    
</template>
<script>
/* eslint-disable */
import PaginateItem from './PaginateItem'
export default {
    name:'CardTable',
    components:{
       PaginateItem
    },
    props: ['providers'],
    data(){
        return{
            arrItem: [],
            sortType: '',
            currentPage: 1,
            itemsPerPage: 10
        }
    },    
    computed:{        
        totalPage(){
            return Math.ceil(this.arrItem.length / this.itemsPerPage)
        },
        minItem(){
            return (this.currentPage * this.itemsPerPage) - this.itemsPerPage
        },
        maxItem(){
            return (this.currentPage * this.itemsPerPage)
        }
    },
    watch:{
        providers(){
            this.currentPage = 1
            this.arrItem = this.providers
        }
    },
    methods: {
        sort(field){
            this.arrItem.sort(this.sortBy(field));
        },
        sortBy(type) {
            if(this.sortType === type) 
                this.arrItem.reverse()
            else
                this.arrItem.sort((a, b) => {
                    const varA = 
                        typeof a[type] === "string" 
                        ? a[type].toLowerCase() 
                        : type == 'sessions_name' 
                        ? a.sessions[0].name 
                        : a[type]
                    const varB = 
                        typeof b[type] === "string" 
                        ? b[type].toLowerCase() 
                        : type == 'sessions_name' 
                        ? b.sessions[0].name 
                        : b[type]

                    return (varA > varB) ? 1 : (varA < varB) ? -1 : 0
                });

            this.sortType = type;
        },
        changePage(numPage){
            this.currentPage = numPage
        }
    },
}
</script>

<style>
    thead td{
        cursor: pointer;
    }
</style>
