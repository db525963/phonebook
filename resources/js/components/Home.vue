<template>
    <div>
        <nav class="panel column is-offset-2 is-8">
            <p class="panel-heading">
                repositories
                <button class="button is-link is-outlined" @click="openAdd">
                    Add new
                </button>
                <span class="is-pulled-right" v-if="loading"><i class="fa fa-refresh fa-spin fa-2x fa-fw"></i></span>
            </p>
            <div class="panel-block">
                <p class="control has-icons-left">
                    <input class="input is-small" type="text" placeholder="search" v-model="searchQuery">
                    <span class="icon is-small is-left">
        <i class="fas fa-search" aria-hidden="true"></i>
      </span>
                </p>
            </div>
            <p class="panel-tabs">
                <a class="is-active">all</a>
                <a>public</a>
                <a>private</a>
                <a>sources</a>
                <a>forks</a>
            </p>
            <a class="panel-block" v-for="item, key in temp">
                <span class="column is-9">
                  {{item.name}}
                </span>
                    <span class="panel-icon column is-1">
                    <i class="has-text-danger fa fa-trash" aria-hidden="true" @click="del(key, item.id)"></i>
                </span>
                    <span class="panel-icon column is-1">
                    <i class="has-text-info fa fa-edit" aria-hidden="true" @click="openUpdate(key)"></i>
                </span>
                    <span class="panel-icon column is-1">
                    <i class="has-text-primary fa fa-eye" aria-hidden="true" @click="openShow(key)"></i>
                </span>
            </a>
            <!--<label class="panel-block">-->
            <!--<input type="checkbox">-->
            <!--remember me-->
            <!--</label>-->
            <div class="panel-block">
                <button class="button is-link is-outlined is-fullwidth">
                    reset all filters<i class="fa fa-trash"></i>
                </button>
            </div>
        </nav>
        <Add :openmodal="addActive" @closeRequest="close"></Add>
        <Show :openmodal="showActive" @closeRequest="close"></Show>
        <Update :openmodal="updateActive" @closeRequest="close"></Update>
    </div>
</template>

<script>
    let Add = require('./Add.vue');
    let Show = require('./Show.vue');
    let Update = require('./Update.vue');

    export default{
        components: {Add, Show, Update},

        data(){
            return {
                addActive : '',
                showActive : '',
                updateActive : '',
                lists: {},
                errors: {},
                loading: false,
                searchQuery : '',
                temp: '',
            }
        },
        watch: {
            searchQuery(){
                // console.log(this.searchQuery);
                if(this.searchQuery.length >0){
                    // let result = this.lists.filter((item)=>{
                    //     return item.name.toLowerCase().indexOf(this.searchQuery.toLowerCase())> -1
                    // });
                    // console.log(result)
                    // // this.lists.filter((index)=>{
                    // //     console.log(index)
                    // // })

                    this.temp = this.lists.filter((item)=>{
                        // return item.name.toLowerCase().indexOf(this.searchQuery.toLowerCase())> -1
                        // console.log(Object.keys(item))
                        // Object.keys(item).some((key)=>{
                        //     let string = item[key]
                        //     console.log(string)
                        // })
                        return Object.keys(item).some((key)=>{
                            let string = String(item[key])
                            return string.toLowerCase().indexOf(this.searchQuery.toLowerCase())> -1
                        })
                    });
                }else{
                    this.temp = this.lists
                }
            }
        },
        mounted(){
            axios.post('/getData')
                .then((response) =>
                        this.lists = this.temp = response.data
                    // console.log(response)
                )
                .catch((error) =>
                        this.errors = error.response.data.errors
                    // console.log(error)
                );
        },
        methods:{
            openAdd(){
                this.addActive = 'is-active';
            },
            openShow(key){
                // console.log(this.$children[1])
                this.$children[1].list = this.temp[key];
                this.showActive = 'is-active';
            },
            openUpdate(key){
                // console.log(this.$children[1])
                this.$children[2].list = this.temp[key];
                this.updateActive = 'is-active';
            },
            close(){
                this.addActive = this.showActive = this.updateActive = '';
            },
            del(key, id){
                this.loading = !this.loading
                if(confirm("Are you sure?")) {
                    axios.delete(`/phonebook/${id}`)
                        .then((response) =>
                            // this.lists = response.data,
                            // console.log('deleted')
                            {this.lists.splice(key,1);this.loading = !this.loading}
                        )
                        .catch((error) =>
                                this.errors = error.response.data.errors
                            // console.log(error)
                        );
                }
            }
        }
    }
</script>
