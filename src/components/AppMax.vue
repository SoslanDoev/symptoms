<template>
    <div class="app-max-component">
      <div class="app-max-component-close">
        <a href="#" class="app-max-component-link" @click.prevent="$emit('close', null)">Закрыть</a>
      </div>
      <h1 class="app-max-component-main-title">Диагноз: <span class="app-max-component-span">{{ sym.diagnosis.toUpperCase() }}</span></h1>
      <h2 class="app-max-component-title"><span class="app-max-component-span">Симптомы:</span></h2>
      <div class="app-max-component-box" v-if="sympList">
        <p v-for="item in sym.symptoms" :key="item">{{ sympList[item].title }}</p>
      </div>
      <h2 class="app-max-component-title"><span class="app-max-component-span">{{ sym.help.about_title }}</span></h2>
      <p class="app-max-component-text">{{ sym.help.about_text }}</p>
      <h2 class="app-max-component-title"><span class="app-max-component-span">{{ sym.help.symp_title }}</span></h2>
      <p class="app-max-component-text">{{ sym.help.symp_text }}</p>
    </div>
  </template>
  
  <script>
    import axios from "axios"
    export default {
      name: "AppMax",
      mounted() {
      const apiUrl_1 = '/symptoms__list.json';
      axios.get(apiUrl_1)
        .then(response => {
          const {data} = response
          this.sympList = data
        })
        .catch(error => {
          console.log(error);
        });
      },
      data: () => ({
        sympList: null
      }),
      props: {
        sym: {
          type: Object,
          required: true,
        }
      },
    }
  </script>
  
  <style scoped>
    .app-max-component {
      color: #000;
      border-radius: 4px;
      position: fixed;
      width: 90vw;
      height: 90vh;
      overflow-y: auto;
      inset: 0;
      z-index: 5;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      display: flex;
      flex-direction: column;
      align-items: center;
      background-color: #fff;
    }
    .app-max-component-close {
      display: flex;
      align-items: flex-end;
      justify-content: flex-end;
      padding: 10px 0;
      width: 70vw;
    }
    .app-max-component-link {
      text-decoration: none;
      color: #000;
      text-align: center;
      transition: 200ms linear;
      margin-left: auto;
    }
    .app-max-component-link:hover {
      color: rgb(0, 104, 74);
      transition: 200ms linear;
    }
    .app-max-component-span {
      color: rgb(0, 104, 74);
    }
    .app-max-component-box {
      display: grid;
      font-weight: 800;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      margin: 0 0 20px;
      text-align: center;
    }
    .app-max-component-main-title,
    .app-max-component-title,
    .app-max-component-text {
      margin: 0 0 20px;
    }
    .app-max-component-text {
      max-width: 70vw;
      text-align: justify;
    }
    @media screen and (max-width: 1055px) {
        .app-max-component {
            width: 100vw;
            height: 100vh;
        }
    }
  </style>