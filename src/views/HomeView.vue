<template>
  <Transition name="fade">
    <AppMax @close="closed" v-if="objElement" :sym="objElement"></AppMax>
  </Transition>
  <div class="wrapper">
    <div class="app-max-main">
      <div class="app-max-symp">
        <h3 class="app-max-label">Выберите свои симптомы</h3>
        <div class="app-max-form">
          <p class="app-max-form-text" v-for="item in symptomsList" :key="item" :class="{'app-max-form-text--active': item.active === true}" @click.prevent="addSymp(item)">{{ item.title }}</p>
          <!-- <textarea class="app-max-textarea" v-model="userSymptoms" id="symptoms"></textarea> -->
        </div>
      </div>
      <div class="app-max-diagnostic">
        <h3 class="app-max-label">Похожие диагнозы</h3>
        <div class="app-max-diagnos" v-if="diagnosis">
          <p class="app-max-diagnostext" v-for="item in diagnosis" :key="item" @click="itemClick(item)">{{ item }}</p>
        </div>
        <p v-else>Пусто</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import AppMax from '../components/AppMax.vue'
export default {
  name: 'App',
  components: {
    AppMax
  },
  mounted() {
    const apiUrl_1 = '/symptoms__list.json';
    const apiUrl_2 = '/symptoms__and__diagnosis.json';
    axios.get(apiUrl_1)
      .then(response => {
        const {data} = response
        this.symptomsList = data
      })
      .catch(error => {
        console.log(error);
      });
    axios.get(apiUrl_2)
      .then(response => {
        const {data} = response
        this.symptomsAndDiagnosis = data
      })
      .catch(error => {
        console.log(error);
      });

    // const filePath = 'symptoms__list.txt';
    // const fileContent = fs.readFileSync(filePath, 'utf8');
    // this.symptomsList = JSON.parse(fileContent);
    // console.log(this.symptomsList)
  },
  data: () => ({
    objElement: null,
    symptomsList: null,
    symptomsAndDiagnosis: null,
    userSymptoms: [], // Симптом который вводит пользователь
    diagnosis: [] // Вывод диагноза на экран
  }),
  watch: {
    userSymptoms() {
      this.findDiagnosis()
    }
  },
  methods: {
    addSymp(item) {
      item.active = !item.active
      if (this.userSymptoms.includes(item)) {
        this.userSymptoms = this.userSymptoms.filter((num) => num !== item)
      } else {
        this.userSymptoms.push(item)
      }
      console.log(this.userSymptoms)
      this.findDiagnosis()
    },
    closed() {
      this.objElement = null
    },
    itemClick(item) {
      let index = -1;
      index = this.symptomsAndDiagnosis.findIndex((element) => element.diagnosis === item);
      this.objElement = this.symptomsAndDiagnosis[index] 
      console.log(this.objElement)
    },
    findDiagnosis() {
      // let symptoms = this.userSymptoms.toLowerCase().split(",") // Переводим текст пользователя в нижний регистр в массив
      let symptoms = this.userSymptoms // Переводим текст пользователя в нижний регистр в массив
      let maxSymptoms = 0 // 
      let indexes = [] // Массив найденных диагнозов
      for (let i = 0; i < this.symptomsAndDiagnosis.length; i++) { // Проходим по массиву диагнозов и симптомов
        const obj = this.symptomsAndDiagnosis[i] // берем i объект
        let count = 0; // Счетчик  
        for (let j = 0; j < obj.symptoms.length; j++) { // Проходим по симптомам в объекте
          if (symptoms.includes(this.symptomsList[obj.symptoms[j]])) { // Проверяем если есть то что ввел пользователь в объекте
          // if (symptoms.includes(obj.symptoms[j])) { // Проверяем если есть то что ввел пользователь в объекте
            count++; // Увеличиваем счетчик 
          }
        }
        // Проверка на нахождение не всех диагнозов с данным симптом
        // а только тех диагнозов у кого больше симптомов
        if (count > maxSymptoms) { 
          maxSymptoms = count; 
          indexes = [i]; 
        } else if (count === maxSymptoms) { 
          indexes.push(i); 
        }
      }
      if (indexes.length > 0) {
        const diagnoses = [];
        for (let index of indexes) {
          diagnoses.push(this.symptomsAndDiagnosis[index].diagnosis);
        }
        this.diagnosis = diagnoses
      } else {
        this.diagnosis = 'Диагноз не найден';
      }
    }
  },
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
* {
  padding: 0;
  margin:0;
  box-sizing: border-box;
}
html, body {
  height: 100%;
}
.wrapper {
  background-image: url("@/assets/bg1.png");
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-attachment: fixed;
  background-repeat: no-repeat;
  background-size: cover;
}
body {
  position: relative;
}
#app {
  font-family: monospace, serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 100%;
  height: 100%;
  margin: auto;
  display: flex;
  justify-content: center;
  padding: 100px 0;
  background-color: rgba(0, 104, 74, 0.25);
}
.app-max-diagnos {
  width: 500px;
  height: 500px;
  overflow-y: auto;
}
.app-max-main {
  display: flex;
  align-items: center;
  gap: 25px;
}
.app-max-form {
  display: flex;
  flex-direction: column;
  width: 500px;
  height: 500px;
  overflow-y: auto;
  border-radius: 4px;
  margin: 0 0 10px;
  outline: none;
  background-color: rgb(0, 104, 74);
  font-size: 18px;
}
.app-max-form-text {
  width: 90%;
  margin: 10px auto;
  padding: 10px;
  font-size: 20px;
  color: #fff;
  transition: 200ms linear;
  position: relative;
  cursor: pointer;
}
.app-max-form-text:hover {
  background-color: rgba(255, 255, 255, .5);
  color: rgb(0, 104, 74);
}
.app-max-form-text--active {
  color: rgb(0, 104, 74);
  border-radius: 4px;
  background-color: #fff;
}
.app-max-label {
  text-align: center;
  color: #fff;
  background-color: rgba(0, 104, 74, 0.8);
  margin: 0 0 10px;
  padding: 10px 0;
  border-radius: 4px;
  width: 100%;
  font-size: 20px;
}
.app-max-textarea {
  width: 40vw;
  height: 500px;
  overflow-y: auto;
  border-radius: 4px;
  border: 5px solid rgb(0, 104, 74);
  margin: 0 0 10px;
  outline: none;
  padding: 10px;
  font-size: 18px;
}
.app-max-submit {
  width: 250px;
  padding: 10px 5px;
  border: none;
  background-color: rgb(0, 104, 74);
  color: #fff;
  border-radius: 4px;
  cursor: pointer;
}
.app-max-diagnostext {
  margin: 5px 0;
  background-color: rgb(0, 104, 74);
  padding: 10px;
  font-size: 20px;
  border-radius: 4px;
  cursor: pointer;
  color: #fff;
  transition: 200ms linear;
}
.app-max-diagnostext:hover {
  background-color: rgb(0, 78, 56);
  transition: 200ms linear;
}
@media screen and (max-width: 1055px) {
  .app-max-main {
    flex-direction: column;
    align-items: baseline;
    width: 900px;
  }
  .app-max-symp {
    width: 100%;
  }
  .app-max-diagnostic {
    width: 100%;
  }
  .app-max-form {
    width: 100%;
  }
  .app-max-diagnos {
    width: 100%;
  }
  .app-max-textarea {
    width: 100%;
  }
  .app-max-textarea {
    height: 300px;
  }
  .app-max-diagnos {
    height: 300px;
  }
}
</style>
