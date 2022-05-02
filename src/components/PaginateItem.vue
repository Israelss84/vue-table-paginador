<template lang="">
    <nav aria-label="Page navigation">
        <ul class="pagination mb-0">
            <li class="page-item"><a class="page-link" href="#" @click="$emit('changePage', 1)"><span aria-hidden="true"><i class="bi bi-chevron-double-left"></i></span></a></li>
            <li class="page-item" :class="[{'disabled': currentPage == 1}]"><a class="page-link" href="#" @click="$emit('changePage',currentPage - 1)"><i class="bi bi-chevron-left"></i></a></li>
            <li v-for="page in pages" :key="page" class="page-item" :class="[{active: currentPage == page}]" >
                <a class="page-link"  href="#" @click="$emit('changePage',page)" >{{page}}</a>
            </li>
            <li class="page-item" :class="[{'disabled': currentPage == totalPage}]"><a class="page-link" href="#" @click="$emit('changePage',currentPage + 1)"><i class="bi bi-chevron-right"></i></a></li>
            <li class="page-item"><a class="page-link" href="#" @click="$emit('changePage', totalPage)"><span aria-hidden="true"><i class="bi bi-chevron-double-right"></i></span></a></li>
        </ul>
    </nav>
</template>
<script>
/* eslint-disable */
export default {
    name: 'PaginateItem',
    emits: ["changePage"],
    props: ['totalPage', 'currentPage', 'maxVisible'],
    computed:{
        pages() {          
            const media = Math.floor(this.maxVisible / 2)        
            const startPage = this.currentPage - media
            const endPage = this.currentPage + media

            const start = (endPage >= this.totalPage) ? (this.totalPage - this.maxVisible + 1) : startPage
            const end = (startPage <= 1) ? this.maxVisible : endPage 

            const range = [];
            for(let page = 1; page <= this.totalPage; page++){
                if( page >= start && page <= end ){
                    range.push(page)
                }
            }

            return range;
        }
    }
}
</script>
<style scoped>
    i{
        font-size: 10px;
    }
</style>