<template>
  <h4>Select Language</h4>
  <p>
    Some of our content is only available in English or Hindi, but we will try
    to honor your choices on this page
  </p>
  <q-option-group
    v-model="selectedLanguage"
    type="radio"
    color="primary"
    :options="languageOptions"
  />
</template>

<script>
import { api } from "boot/axios";
import { useLanguageStore } from "stores/LanguageStore";
export default {
  name: "HinduLanguages",
  setup() {
    const languageStore = useLanguageStore();
    return {
      languageStore,
    };
  },
  data() {
    return {
      languageOptions: [],
      languageArray: [],
      selectedLanguage: this.languageStore.getLanguageSelected,
    };
  },
  watch: {
    selectedLanguage: {
      handler() {
        console.log(this.selectedLanguage);
        this.languageStore.updateLanguageSelected(this.selectedLanguage);
      },
      deep: true,
    },
  },
  created() {
    api.get("api/hindi/languages").then((response) => {
      this.languageArray = response.data;
      this.languageStore.updateLanguages(this.languageArray);
      this.languageOptions = this.languageArray.map((item) => ({
        label: item.name,
        value: item.languageCodeHL,
      }));
    });
  },
};
</script>
