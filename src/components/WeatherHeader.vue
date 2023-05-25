<template>
    <header class="weather-header">
      <div class="weather-header__container">
        <h1 class="weather-header__title">Weather App</h1>
        <div class="weather-header__location">
          <p class="weather-header__location-text">{{ location }}</p>
        </div>
      </div>
  <!--Doesn't work-->
      <div v-if="showModal" class="location-modal">
        <div class="location-modal__content">
          <h2 class="location-modal__title">Select Location</h2>
          <input class="location-modal__input" type="text" v-model="newLocation">
          <div class="location-modal__buttons">
            <button class="location-modal__button location-modal__button--cancel" @click="closeLocationModal">Cancel</button>
            <button class="location-modal__button location-modal__button--save" @click="saveLocation">Save</button>
          </div>
        </div>
      </div>
    </header>
  </template>
  
  <script>
  export default {
    name: 'WeatherHeader',
    model: {
      prop: 'location',
      event: 'change'
    },
    props: {
      location: {
        type: String,
        default: ''
      }
    },
    data() {
      return {
        showModal: false,
        newLocation: this.location
      };
    },
    methods: {
      openLocationModal() {
        this.showModal = true;
      },
      closeLocationModal() {
        this.showModal = false;
      },
      saveLocation() {
        if (this.newLocation) {
          this.$emit('change', this.newLocation);
          this.showModal = false;
          this.newLocation = '';
        }
      }
    }
  };
  </script>
  
  <style scoped>
  
  .location-modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .location-modal__content {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
  }
  
  .location-modal__title {
    margin-top: 0;
  }
  
  .location-modal__input {
    width: 100%;
    margin-bottom: 10px;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .location-modal__buttons {
    display: flex;
    justify-content: flex-end;
  }
  
  .location-modal__button {
    margin-left: 8px;
    padding: 6px 12px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .location-modal__button--cancel {
    background-color: #ccc;
  }
  
  .location-modal__button--save {
    background-color: #3498db;
    color: #fff;
  }
  </style>
  