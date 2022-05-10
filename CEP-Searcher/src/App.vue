<script>
import RenderCepData from './components/RenderCepData.vue'

export default {
  props: {
    searchInput: Object,
    searchButton: Object
  },
  data() {
    return {
      tagElements: [
        {searchInput: null},
        {searchButton: null}
      ],
      getCep:    false,
      cepNumber: null,
      cepData:   null
    }
  },
  methods: {
    async getCepDataByCepNumber() {
      this.cepNumber = (this.cepNumber == null ? this.tagElements.searchInput.value : this.cepNumber);
      const res = await fetch(
        `https://viacep.com.br/ws/${this.cepNumber}/json/`
      )
      this.cepData = await res.json();
      if(this.cepData == null){
        this.displayErrorMessages('invalid cep, nothing found');
        return;
      }
      this.getCep = true;
    },
    getDataAndView() {
      if(this.tagElements.searchInput.value == ''){
        this.displayErrorMessages('a valid cep is required');
        this.getCep = false;
        return;
      }
      if(this.tagElements.searchInput.value.length < 8){
        this.displayErrorMessages('too small, 8 numbers are required');
        this.getCep = false;
        return;
      }
      this.getCepDataByCepNumber()
    },
    formatCepData() {
      var enteredValue  = this.tagElements.searchInput.value;
      if(enteredValue.length > 8){
        this.tagElements.searchInput.value = this.cepNumber;
        return;
      }
      var formatedValue = enteredValue.replace(/\D/g, "");
      this.tagElements.searchInput.value = formatedValue;
      this.cepNumber = formatedValue;
    },
    loadEssentialTags() {
      this.tagElements.searchInput = document.getElementById('search-cep-data');
      this.tagElements.searchInput.addEventListener("input", this.formatCepData);

      this.tagElements.searchButton = document.getElementById('search-for-cep');
      this.tagElements.searchButton.addEventListener("click", this.getDataAndView);

      document.getElementById('close-message').addEventListener("click", this.closeMessages);
    },
    displayErrorMessages(message) {
      document.getElementById('messages').classList.remove('hidden');
      document.getElementById('messages').value = message;
    },
    closeMessages() {
      document.getElementById('messages').value = '';
      document.getElementById('messages').classList.add('hidden');
    }
  },
  mounted() {
    this.loadEssentialTags()
  },
  components: {
    RenderCepData
  }
}
</script>

<template>
  <div id="messages" class="alert alert-danger d-flex justify-content-between mt-4 hidden" role="alert">
    <p class="mt-auto mb-auto">
      Message here...
    </p>
    <a id="close-message" class="cursor-pointer mt-auto mb-auto">
      x
    </a>
  </div>

  <section class="mt-5 p-3 border shadow">
    <p class="h5 text-center border-bottom pb-3">
      Welcome to the best CEP searcher web tool ever!
    </p>
    <p class="h6 mt-5 mb-3">
      Before starting using our service please consider reading our terms of use:
    </p>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis finibus, ante id mattis pretium,
       diam ex gravida justo, ac tempus libero diam iaculis lacus. Cras sapien ipsum, consequat nec arcu sit amet, 
       lobortis tincidunt nulla. Pellentesque feugiat diam ut dolor pretium convallis. Proin suscipit tempus laoreet.
        Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Suspendisse eget 
        elit ac velit sollicitudin porta. Maecenas dapibus ligula id ipsum euismod bibendum. Etiam tristique placerat justo id fringilla. 
        Vivamus suscipit justo sit amet odio cursus viverra. Ut efficitur justo vitae orci aliquet, at volutpat sapien rhoncus. Nulla eget dictum erat. 
        Curabitur sollicitudin mauris quam, vel pellentesque lacus posuere eu. 
    </p>
  </section>

  <RenderCepData v-if="getCep" :cepData="this.cepData" />
</template>

<style>
  .hidden{
    display: none!important;
  }
  .cursor-pointer{
    cursor: pointer;
  }
</style>