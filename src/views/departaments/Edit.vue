<template>
  <div class="uk-container">
    <DepartamentsForm
      :hideSubmit="false"
      :hideRelation="false"
      :form="departament"
      @submit="onSubmit"
      :title="'Edytuj oddział'"
      :errors="errors"
    ></DepartamentsForm>
  </div>
</template>

<script>
import DepartamentsForm from "../../components/departaments/Form";
import axios from "axios";

export default {
  data() {
    return {
      departament: null,
      errors: {}
    };
  },
  components: {
    DepartamentsForm,
  },
  beforeMount() {
    this.getDepartament();
  },
  methods: {
    getDepartament: async function () {
      const response = await axios.get(
        process.env.VUE_APP_API_SERVER+"/api/web/departament/" + this.$route.params.id
      );
      this.departament = response.data;
    },
    onSubmit: async function (data) {
      try {
        const response = await axios.put(
          process.env.VUE_APP_API_SERVER+"/api/web/departament/" + this.$route.params.id,
          data
        );
        if (response.data) {
          this.$router.push({name: "DepartamentsDetails", params: {id: this.departament.id}})
        }
      } catch (error) {
          this.errors = error.response.data.errors;
      }
    },
  },
};
</script>

<style>
</style>