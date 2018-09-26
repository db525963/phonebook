<template>
    <div class="modal" :class="openmodal">
        <div class="modal-background"></div>
        <div class="modal-card">
            <header class="modal-card-head">
                <p class="modal-card-title">Modal title</p>
                <button class="delete" aria-label="close" @click="close"></button>
            </header>
            <section class="modal-card-body">
                <div class="field">
                    <label class="label">Name</label>
                    <div class="control">
                        <p>{{list.name}}</p>
                    </div>
                </div>
                <div class="field">
                    <label class="label">Phone</label>
                    <div class="control">
                        <p>{{list.phone}}</p>
                    </div>
                </div>
                <div class="field">
                    <label class="label">Email</label>
                    <div class="control">
                        <p>{{list.email}}</p>
                    </div>
                </div>
                <li class="panel-block">
                    <label class="column is-2">Name</label>{{list.name}}
                </li>
                <li class="panel-block">
                    <label class="column is-2">Phone</label>{{list.phone}}
                </li>
                <li class="panel-block">
                    <label class="column is-2">Email</label>{{list.email}}
                </li>
            </section>
            <footer class="modal-card-foot">
                <button class="button is-success" @click="save">Save changes</button>
                <button class="button" @click="close">Cancel</button>
            </footer>
        </div>
    </div>
</template>

<script>
    export default{
        props: ['openmodal'],
        data(){
            return{
                list: '',
            }
        },
        methods: {
            close(){
                this.$emit('closeRequest');
            },
            save(){
                axios.post('/phonebook', this.$data.list)
                    .then((response) =>
                            this.close()
                        // console.log(response)
                    )
                    .catch((error) =>
                            this.errors = error.response.data.errors
                        // console.log(error)
                    );
            },
        }
    }
</script>
