<template>
    <div>
        <nav class="panel column is-offset-2 is-8">
            <p class="panel-heading">
                repositories
                <button class="button is-link is-outlined" @click="openAdd">
                    Add new
                </button>
            </p>
            <div class="panel-block">
                <p class="control has-icons-left">
                    <input class="input is-small" type="text" placeholder="search">
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
            <a class="panel-block">
            <span class="column is-9">
              bulma
            </span>
                <span class="panel-icon column is-1">
                <i class="has-text-danger fa fa-trash" aria-hidden="true"></i>
            </span>
                <span class="panel-icon column is-1">
                <i class="has-text-info fa fa-edit" aria-hidden="true"></i>
            </span>
                <span class="panel-icon column is-1">
                <i class="has-text-primary fa fa-eye" aria-hidden="true"></i>
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
    </div>
</template>

<script>
    let Add = require('./Add.vue');
    export default{
        components: {Add},

        data(){
            return {
                addActive : '',
            }
        },
        methods:{
            openAdd(){
                this.addActive = 'is-active';
            },
            close(){
                this.addActive = '';
            }
        }
    }
</script>
