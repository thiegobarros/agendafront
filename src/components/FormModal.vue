<template>
    <div>
        <b-modal id="form-modal" title="Contato" @ok.prevent="submit" @hidden="form = {}" ok-title="Salvar" ok-variant="success" ok-only>
            <b-form>
                <b-form-group id="input-group-1" label="Nome:" label-for="input-1">
                    <b-form-input
                    id="input-2"
                    v-model="form.nome"
                    required
                    placeholder="Nome Completo"
                    ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-2" label="Sexo:" label-for="input-2">
                    <b-form-input
                    id="input-2"
                    v-model="form.sexo"
                    required
                    placeholder="M ou F"
                    ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-3" label="Telefone:" label-for="input-3">
                    <b-form-input
                    id="input-3"
                    v-model="form.telefone"
                    required
                    placeholder="(99) 99999-9999"
                    ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-4" label="Email:" label-for="input-4">
                    <b-form-input
                    id="input-4"
                    v-model="form.email"
                    type="email"
                    required
                    placeholder="email@email.com"
                    ></b-form-input>
                </b-form-group>
            </b-form>
        </b-modal>
    </div>
</template>

<script>
    export default {
        name: 'FormModal',
        data() {
            return {
                form: {},
                action: {}
            }
        },
        methods: {
            create() {
                this.action = {
                    url: 'agenda',
                    method: 'post'
                };
                this.$bvModal.show('form-modal');
            },
            async edit(id) {
                try {
                    const {data} = await axios.get(`agenda/${id}`);
                    this.form = data;
                    this.$bvModal.show('form-modal');
                    this.action = {
                        url: 'agenda',
                        method: 'put'
                    };
                } catch (e) {
                    console.log(e)
                }
                
            },
            async remove(contato) {
                try {
                    const {data} = await axios({
                        method: 'delete',
                        url: 'agenda',
                        data: contato
                    });
                    this.$emit('success');
                } catch(e) {
                    console.log(e)
                }
            },
            async submit() {
                try{
                    const {data} = await axios({
                        method: this.action.method,
                        url:this.action.url,
                        data: this.form
                    });
                    this.$emit('success');
                    this.$nextTick(()=> this.$bvModal.hide('form-modal'));
                }catch(e){
                    console.log(e)
                }
            },
        }
    }
</script>

<style>

</style>