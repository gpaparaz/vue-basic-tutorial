<script>
import { ref, computed, watch, watchEffect, provide } from 'vue'
import { reactive } from 'vue'
import GreetingMessage from './components/GreetingMessage.vue'
import MainMenu from './components/MainMenu.vue'

export default {
  components: { GreetingMessage, MainMenu },

  setup() {
    // access props, attributes
    // slots and emit events

    // props: Data that’s passed down from a parent component.
    // context: A Javascript object that exposes 3 different component properties. Namely attributes , slots and emit events.


    const saluti = "Hello there"

    // define ref
    const refValue = ref('Hello from ref value')

    //esempio in cui si usare ref con un oggetto composto
    const complexObj = ref({ msg: "Hello", user: 'Gio' })

    const reactiveObj = reactive({ type: "Fiat", model: 'Ritmo' }) //Nota: Ma ref ha la proprietà value per la riassegnazione, reactive no e quindi non può essere riassegnato

    //////////////////////////////////////////////////////
    const helloworld = ref('Hello World')
    //funzioni
    // regular function
    function newGreeting1() {
      helloworld.value = 'Hello John'
    }

    // anonymous function
    const newGreeting2 = function () {
      helloworld.value = 'Hello Jane'
    }

    // arrow function
    const newGreeting3 = () => {
      helloworld.value = 'Hello Jack'
    }

    //////////////////////////////////////////////////////
    //With the Composition API, we define computed properties with the computed function. The function is imported from the ‘vue’ package and takes a callback function as an argument.
    const user = ref({ firstName: '', lastName: '' })
    // functions to set first & last name
    const setFirstName = ($event) => {
      user.value.firstName = $event.target.value
    }
    const setLastName = ($event) => {
      user.value.lastName = $event.target.value
    }
    // computed
    const hellouser = computed(() => {
      return 'Hello ' + user.value.firstName + ' ' + user.value.lastName
    })

    ///////////////////////////////////////////////////
    //two way databindig
    const utente = ref({ firstName: '', lastName: '' })

    //////////////////////////////////////////////////////
    // data to watch
    const message = ref('')

    watch(message, (newValue, oldValue) => {
      //commento perchè intasa la console
      //console.log('New Value', newValue)
      //console.log('Old Value', oldValue)
    })

    //Like the watch option, if we want to run the watcher on the initial value as well, we can specify immediate to be true as an optional third argument.
    // watch(dataSource, (newValue, oldValue) => {
    //   console.log('New Value', newValue)
    //   console.log('Old Value', oldValue)
    // }, { immediate: true })
    //If we want to watch multiple data sources, we can specify them in an array. When we do, the newValue and oldValue also become arrays as newValues and oldValues .
    // const firstName = ref('')
    // const lastName  = ref('')
    // watch(
    //   [firstName, lastName],
    //   (newValues, oldValues) => {
    //     console.log('New First Name', newValues[0])
    //     console.log('Old First Name', oldValues[0])
    //     console.log('New Last Name',  newValues[1])
    //     console.log('Old Last Name',  oldValues[1])
    // })
    // return { firstName, lastName }

    //If our ref is an object instead of primitives, we have to use a getter callback function to accesss the value.
    const userRefWatch = ref({ firstName: '', lastName: '' })

    watch(
      [
        // getter callbacks
        () => userRefWatch.value.firstName,
        () => userRefWatch.value.lastName
      ],
      (newValues, oldValues) => {
        // console.log('New First Name', newValues[0])
        // console.log('Old First Name', oldValues[0])
        // console.log('New Last Name',  newValues[1])
        // console.log('Old Last Name',  oldValues[1])
      })

    ///////////////////////////////////////////////
    //watchEffect function
    //works the same as watch , except for two things:
    // It runs immediately on the initial value, the same as when immediate:true in watch .
    // It automatically tracks dependencies in its body, we don’t explicitly add them as arguments.
    const messageWatchEffect = ref('Initial message')

    watchEffect(() => {
      // we must use data in the body in
      // some way for Vue to track it
      // console.log(messageWatchEffect.value)
    })

    ////////////////////////////////////////////////

    //PARENT CHILD
    const userParent = ref({ firstName: '', lastName: '' })
    const isMenuOpen = ref(false)

    // provide 'username'
    provide('username', 'John')


    return {
      greeting: saluti, //nel template cerca un oggetto che si chiama greeting, che è assegnato ad un oggetto di nome saluti, di tipo stringa

      // make ref available to the template, so i return it
      refName: refValue,

      compObj: complexObj,
      //se io espongo l'intero oggetto così, posso modificare le sue proprietà attraverso funzioni.
      //se invece lo spacchetto ritornando ad esempio complexMsg: complexObj.value.msg allora non posso modificare i suoi valori


      //ritorno l'oggetto reactive
      reactObj: reactiveObj,

      //////////////////////////////////////////////////////
      //funioni
      helloworld,
      newGreeting1,
      newGreeting2,
      newGreeting3,

      //////////////////////////////////////////////////////
      //computed
      user,
      hellouser,
      setFirstName,
      setLastName,

      //////////////////////////////////////////////////////
      utente,
      message,
      userRefWatch,
      messageWatchEffect,
      ///////////////////////////////////////////////////////
      userParent,
      isMenuOpen
    }
  }
}

</script>

<template>
  <h1>Hello world</h1>
  <p>{{ greeting }}</p>
  <br>
  <p>{{ refName }}</p>
  <br>
  <p>Mostro a video i valori di un oggetto con attributi: {{ compObj.msg }}, {{ compObj.user }}</p>
  <br>
  <p>Mostro i valori di un oggetto usando reactive: {{ reactObj.type }}, {{ reactObj.model }}</p>
  <br>
  <p>Uso tre funzioni per cambiare il valore di una ref. utilizzo tre bottoni</p>
  <p><button @click="newGreeting1">Regular function</button></p>
  <p><button @click="newGreeting2">Anonymous function</button></p>
  <p><button @click="newGreeting3">Arrow function</button></p>
  <p>{{ helloworld }}</p>
  <br>
  <p>Uso di computed. Sostanzialmente fa il two-way-databinding tra l'inputtext e l'oggetto user usando una funzione per
    settare il valore</p>
  <p>{{ hellouser }}</p>
  <input type="text" placeholder="First Name" @input="setFirstName">
  <input type="text" placeholder="Last Name" @input="setLastName">
  <br>
  <br>
  <p>Esempio di two way databinding</p>
  <p>Hello {{ utente.firstName }} {{ utente.lastName }}</p>
  <input type="text" placeholder="First Name" v-model="utente.firstName">
  <input type="text" placeholder="Last Name" v-model="utente.lastName">
  <br>
  <br>
  <p>Watch: watch è una funzione che permette di di controllare lo stato di un dato. La funzione accetta un newValue e
    oldValue </p>
  <p>{{ message }}</p>
  <input type="text" placeholder="Message" v-model="message">
  <br>
  <br>
  <p>Hello {{ userRefWatch.firstName }} {{ userRefWatch.lastName }}</p>
  <input type="text" placeholder="First Name" v-model="userRefWatch.firstName">
  <input type="text" placeholder="Last Name" v-model="userRefWatch.lastName">
  <br><br>

  <p>Watcheffect function</p>
  <p>{{ messageWatchEffect }}</p>
  <input type="text" placeholder="Message" v-model="messageWatchEffect">
  <br>
  <br>
  <br>
  <h1>Parent-Child</h1>

  <br>
  <p>Quando uso props, devo definire come si chiama ciò che comunicheranno parent e child </p>
  <greeting-message :firstName="userParent.firstName" :lastName="userParent.lastName" />
  <input type="text" placeholder="First Name" v-model="userParent.firstName">
  <input type="text" placeholder="Last Name" v-model="userParent.lastName">

  <br>
  <br>
  <br>

  <p>Custom event and context: definisco un evento nel child, e lo invio al parent tramite $emit function</p>
  <p>The parent listens to the event with event binding on the child component. We can then specify some sort of
    functionality to be executed when the event is captured, like changing a data property.</p>
  <br>
  <p>Essendo che una volta aperto il menu non è stato inserito nulla, questo evento mi va solo ad interagire con
    isMenuOpen. Quando è aperto isMenuOpen è true, quando clicco su closeMenu, isMenuOpen diventa false</p>
  <button @click="isMenuOpen = true">Open Menu</button>
  <main-menu v-show="isMenuOpen" @closeMenu="isMenuOpen = false" />
  <br>
  <br>

  <p>Provide & inject</p>
  <p>Con provide io voglio trasmettere info dal parent al child </p>
  <greeting-message />
  <br>
  <br>
  <br>
  <br>
  <p>fine pagina</p>
</template>

<style scoped>

</style>
