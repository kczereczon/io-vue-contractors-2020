<template>
  <div class="uk-card uk-margin-top uk-card-default uk-card-body">
    <h3 class="uk-card-title">{{ title }}</h3>
    <vk-grid>
      <div class="uk-width-1-1">
        <label
          :class="{
            'uk-form-danger': this.getError('name'),
          }"
          >Nazwa</label
        >
        <input
          :class="{
            'uk-input': true,
            'uk-form-danger': this.getError('name'),
          }"
          type="text"
          placeholder="Nazwa oddziału"
          v-model="form.name"
          @keypress="removeError('name')"
        />
        <p v-for="item in getError('name')" :key="item" class="uk-text-danger">
          {{ item }}
        </p>
      </div>
      <div class="uk-width-1-3@s">
        <label
          :class="{
            'uk-form-danger': this.getError('street'),
          }"
          >Ulica i numer budynku</label
        >
        <input
          :class="{
            'uk-input': true,
            'uk-form-danger': this.getError('street'),
          }"
          type="text"
          placeholder="Ulica wraz z numerem budynku"
          v-model="form.street"
          @keypress="removeError('street')"
        />
        <p v-for="item in getError('street')" :key="item" class="uk-text-danger">
          {{ item }}
        </p>
      </div>
      <div class="uk-width-1-4@s">
        <label
          :class="{
            'uk-form-danger': this.getError('city'),
          }"
          >Miasto</label
        >
        <input
          :class="{
            'uk-input': true,
            'uk-form-danger': this.getError('city'),
          }"
          type="text"
          placeholder="Miasto oddziału"
          v-model="form.city"
          @keypress="removeError('city')"
        />
        <p v-for="item in getError('city')" :key="item" class="uk-text-danger">
          {{ item }}
        </p>
      </div>
      <div class="uk-width-1-4@s">
        <label
          :class="{
            'uk-form-danger': this.getError('postal_code'),
          }"
          >Kod pocztowy</label
        >
        <input
          :class="{
            'uk-input': true,
            'uk-form-danger': this.getError('postal_code'),
          }"
          type="text"
          placeholder="Kod pocztowy"
          v-model="form.postal_code"
          @keypress="removeError('postal_code')"
        />
        <p v-for="item in getError('postal_code')" :key="item" class="uk-text-danger">
          {{ item }}
        </p>
      </div>
      <div class="uk-width-1-6@s">
        <label
          :class="{
            'uk-form-danger': this.getError('country'),
          }"
          >Kraj</label
        >
        <input
          :class="{
            'uk-input': true,
            'uk-form-danger': this.getError('country'),
          }"
          type="text"
          placeholder="Kraj oddziału"
          v-model="form.country"
          @keypress="removeError('country')"
        />
        <p v-for="item in getError('country')" :key="item" class="uk-text-danger">
          {{ item }}
        </p>
      </div>
      <div v-if="!hideRelation" class="uk-width-1-1@s">
        <label
          :class="{
            'uk-form-danger':
              this.getError('contractor_id'),
          }"
          >Kontrahent</label
        >
        <select
          :class="{
            'uk-select': true,
            'uk-form-danger': this.getError('contractor_id'),
          }"
          type="text"
          placeholder="50"
          v-model="form.contractor_id"
           @change="removeError('contractor_id')"
        ><option
            v-for="contractor in contractors"
            :key="contractor"
            :value="contractor.id"
        >{{contractor.name}}</option></select>
        <p v-for="item in getError('contractor_id')" :key="item" class="uk-text-danger">
          {{ item }}
        </p>
      </div>
    </vk-grid>
      <div v-if="!hideSubmit" class="uk-margin">
        <div class="uk-form-controls">
          <vk-button @click="submit">ZAPISZ</vk-button>
        </div>
      </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: {
    title: String,
    errors: { type: Object, default: () => {} },
    form: {
      type: Object,
      default: () => ({
        city: null,
        street: null,
        postal_code: null,
        country: null,
        name: null,
        contractor_id: null,
      }),
    },
    hideSubmit: { type: Boolean, default: () => true },
    hideRelation: { type: Boolean, default: () => true },
  },
  created: function () {
    this.getContractors();
    this.$parent.$on("submit", this.submit);
  },
  data: function(){
    return {
      contractors: {}
    }
  },
  methods: {
    submit: function () {
      this.$emit("submit", this.form);
    },
    getError: function (field) {
      return this.errors[field] || this.errors["departament." + field];
    },
    removeError: function(field) {
      if(this.errors[field]) {
        delete this.errors[field];
      } else if(this.errors["departament." + field]) {
        delete this.errors["departament." + field];
      }
    },
    getContractors: async function(){
      const response = await axios.get(
        process.env.VUE_APP_API_SERVER+"/api/contractor/");
      this.contractors = response.data;
    }
  },
};
</script>

<style>
</style>