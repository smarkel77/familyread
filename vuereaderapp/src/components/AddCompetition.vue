<template>
   <v-form v-model="valid">
      <v-container>
        <v-layout>
          <v-flex
            xs12
            md4
          >
          <div>
    <input type="text" v-model="nameOfCompetition" placeholder="Name your competition"/>
          </div>
          <div>
    <input v-model="startDate" type="date" placeholder="start date"/>
    </div>
          <div>
    <input v-model="endDate" type="date" placeholder="end date"/>
    </div>
          <div>
    <input type="text" v-model="description" placeholder="describe your competition"/>
    </div>
          <div>
    <input type="number" v-model="minutesInCompetition" placeholder="enter minutes in competition"/>
    </div>
          <div>
    <v-btn color="primary" dark @click="validate">Add New competition</v-btn>
    </div>
          <div>
    <v-btn color="secondary" light @click="cancel">Cancel</v-btn>
    </div>
   </v-flex>
        </v-layout>
      </v-container>
    </v-form>
</template>

<script>
import axios from 'axios'
import EventBus from './EventBus'
export default {
  data () {
    return {
      nameOfCompetition: null,
      startDate: new Date().toISOString().substring(0, 10),
      endDate: null,
      description: '',
      minutesInCompetition: null,
      familyId: this.$session.get('familyId')
    }
  },
  computed: {
    familyParams () {
      const params = new URLSearchParams()
      params.append('familyId', this.familyId)
      return params
    }
  },
  methods: {
    addCompetition () {
      axios({
        method: 'post',
        url: 'http://localhost:8080/capstone/addcompetition',
        data: {
          nameOfCompetition: this.nameOfCompetition,
          startDate: this.startDate,
          endDate: this.endDate,
          description: this.description,
          minutesToFinish: this.minutesInCompetition,
          familyId: this.familyId
        }
      }).then(response => {
        console.log(response)
        EventBus.$emit('rebootCompetitionList', true)
      }).catch(e => console.log(e))
    },
    validate () {
      console.log('validate')
      if (this.nameOfCompetition || this.description || this.minutesInCompetition !== '') {
        console.log('validated')
        this.addCompetition()
        EventBus.$emit('competitionAdded', true)
        this.returnToHomepage()
      }
    },
    cancel () {
      EventBus.$emit('toggleAddCompetition', false)
    },
    returnToHomepage () {
      EventBus.$emit('toggleAddCompetition', false)
    }
  }
}
</script>

<style>
input {
font-weight: bolder;
}
</style>
