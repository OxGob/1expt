<template>
  <q-page>
      <q-tabs color="secondary" glossy align="justify">
        <q-tab slot="title" name="generalities" icon="subject" label="Generalities" />
        <q-tab slot="title" name="criteria" icon="fingerprint" label="Inclusion Criteria" />
        <q-tab slot="title" name="test" icon="verified_user" label="Tab Disease"/>
        <q-tab slot="title" default name="test2" icon="verified_user" label="Tab Inst"/>
        <!-- General Tab -->
        <q-tab-pane name="generalities">Gen tab
          <!-- General Information Tab Card -->
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-title>Generalities
              <span slot="subtitle">General information about the study</span>
            </q-card-title>
            <q-card-main>
              <q-field label="Title" helper="Short name of the study">
               <q-input v-model="studyTitle" @input="$v.studyTitle.$touch()" clearable />
                <div v-if="$v.studyTitle.$dirty">
                  <p v-if="!$v.studyTitle.required" class="q-mt-sm text-negative"> A title is required for the study. Please enter it.</p>
                </div>
              </q-field>
              <q-field label="Description" helper="Layman's description of the study">
                <q-input v-model="studyDescription" @input="$v.studyDescription.$touch()" type="textarea" rows="7" clearable />
                 <div v-if="$v.studyDescription.$dirty">
                  <p v-if="!$v.studyDescription.required" class="q-mt-sm text-negative">A description is required for the study. Please enter it.</p>
                 </div>
              </q-field>
            </q-card-main>
          </q-card>
          <!-- Gen Info Tab: Principal Investigator Card -->
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-title>Principal Investigator(s)
              <span slot="subtitle">Information concerning the principal investigators:</span>
            </q-card-title>
            <q-card-main>
              <div v-for="(principalInvestigator, index) in principalInvestigators" :key="principalInvestigator.id">
                <div>
                  <q-btn class="vertical-top" v-show="index !==0" round size="sm" color="negative" icon="remove" @click="removeRowInvestigator(index)" />
                </div>
                <q-field label="Name & Title:" helper="Principal Investigator Name & Title">
                  <q-input v-model="principalInvestigator.nameTitleOfInvestigator" type="text" clearable />
                </q-field>
                <q-field label="Contact:" helper="Contact Details, may include address">
                  <q-input v-model="principalInvestigator.contactDetailsOfInvestigator" type="textarea" rows="4" clearable />
                </q-field>
                <q-field label="Institution:" helper="Name of Institution involved in study">
                  <q-input v-model="principalInvestigator.institutionOfInvestigator" type="textarea" rows="2"  clearable />
                </q-field>
                <q-btn class="float-right q-mt-sm" round size="sm" color="primary" icon="add" @click="addRowInvestigator(index)" />
                <q-card-separator class="q-mb-md q-mt-xl"/>
              </div>
            </q-card-main>
          </q-card>
          <!-- Gen Info Tab: Institutions Card -->
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-title>Institution(s)
              <span slot="subtitle">Institutions that the data might or might not be shared with:</span>
            </q-card-title>
            <q-card-main>
              <div v-for="(institution, index) in institutions" :key="institution.id">
                <q-btn class="vertical-top" v-show="index !==0" round size="sm" color="negative" icon="remove" @click="removeRowInstitution(index)" />
                <q-field label="Name of Institution:" helper="Please enter the name of the institution">
                  <q-input v-model="institution.nameOfInstitution" type="text" clearable />
                </q-field>
                <q-field label="Contact:" helper="Contact Details of the institution, may include address">
                  <q-input v-model="institution.contactOfInstitution" type="textarea" rows="4" clearable />
                </q-field>
                <q-field class="q-mt-md" label="Data Access:" helper="Data access per institution. Please select the level of access to grant or not.">
                  <q-radio v-model="institution.radioDataAccessInstitution" val="no" color="secondary" label="NO" />
                  <q-radio class="q-ml-md q-mr-md" v-model="institution.radioDataAccessInstitution" val="anon" color="anon" label="ANON" />
                  <q-radio v-model="institution.radioDataAccessInstitution" val="full" color="full" label="FULL" />
                </q-field>
                <q-btn class="float-right q-mt-sm" round size="sm" color="primary" icon="add" @click="addRowInstitution(index)" />
                <q-card-separator class="q-mb-md q-mt-xl"/>
              </div>
            </q-card-main>
          </q-card>
          <!-- Gen Info Tab: Date Card -->
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-title>Dates Of Study
              <span slot="subtitle">The Start and End Dates of the study</span>
            </q-card-title>
            <q-card-main>
              <div>
                <div>
                  <div class="row gutter-lg">
                    <div class="col-xs-4 col-md-4">
                        <q-field label="Start Date" />
                        </div>
                        <div class="col-xs-4 col-md-4">
                          <q-datetime class="q-ml-xl q-mb-lg" v-model="dateStart" @input="$v.dateStart.$touch()" type="date" format="D-MMM-YYYY" clearable />
                          <div v-if="$v.dateStart.$dirty">
                            <p v-if="!$v.dateStart.required" class="q-mt-sm text-negative">The Start Date is required. Please enter it.</p>
                          </div>
                        </div>
                    </div>
                  </div>
              </div>
              <div>
                <div>
                  <div class="row gutter-lg">
                    <div class="col-xs-4 col-md-4">
                        <q-field label="End Date" />
                        </div>
                        <div class="col-xs-4 col-md-4">
                          <q-datetime class="q-ml-xl" v-model="dateEnd" type="date" @input="checkEndDate(dateStart, dateEnd)" format="D-MMM-YYYY" clearable />
                        </div>
                    </div>
                  </div>
              </div>
            </q-card-main>
          </q-card>
          <!-- Gen Info Tab: Submit Test Button -->
          <q-btn color="negative" class="float-right"  size="lg" label="SUBMIT" @click="submitButtonClicked">
          </q-btn>
        </q-tab-pane>
                <!-- Criteria Tab -->
        <q-tab-pane name="criteria">
          <!-- Inclusion Criteria Tab Card -->
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-title>Inclusion Criteria
              <span slot="subtitle">Criteria to filter participants in the study</span>
            </q-card-title>
            <q-card-main>
              <div>
                <div>
                <!-- Age Range & Sex -->
                  <div class="row gutter-lg">
                    <div class="col-xs-4 col-md-4">
                        <q-field label="Age Range"/>
                    </div>
                    <div class="col-xs-4 col-md-4">
                        <q-input v-model="ageRangeMin" type="number" min="0" oninput="validity.valid||(value='')" placeholder="Minimum Age of participants" clearable/>
                         <div v-if="!$v.ageRangeMin.between" class="q-mt-sm text-negative"> The age is between 0 and 140.</div>
                    </div>
                    <div class="col-xs-4 col-md-4">
                        <q-input v-model="ageRangeMax" type="number" min="0" oninput="validity.valid||(value='')" placeholder="Maximum Age of participants" @input="checkMaxAge(ageRangeMin, ageRangeMax)" clearable/>
                          <div v-if="!$v.ageRangeMax.between" class="q-mt-sm text-negative"> The age is between 0 and 140.</div>
                    </div>
                    <div class="col-xs-4 col-md-4">
                      <q-field label="Sex" />
                    </div>
                    <div class="col-xs-4 col-md-4">
                      <q-checkbox class="q-mr-lg" v-model="checkArrayGender" label="M" color="secondary" val="genderMale" />
                      <q-checkbox class="q-mr-lg" v-model="checkArrayGender" label="F" color="secondary" val="genderFemale" />
                      <q-checkbox v-model="checkArrayGender" label="OTHER" color="secondary" val="genderOther" />
                    </div>
                  </div>
                </div>
              </div>
            </q-card-main>
          </q-card>
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-main>
              <div>
                <div>
                  <!-- Disease, Lifestyle and Meds -->
                  <div class="row gutter-lg">
                    <div class="col-xs-4 col-md-4">
                      <q-field label="Disease Choice" />
                    </div>
                    <div class="col-xs-4 col-md-6">
                      <q-input type="text" id="disease-form" placeholder="Disease" />
                    </div>
                    <div class="col-xs-4 col-md-4">
                      <q-field label="Lifestyle" />
                    </div>
                    <div class="col-xs-4 col-md-6">
                      <q-radio v-model="radio_lifestyle" val="active" color="secondary" label="Active" />
                      <q-radio v-model="radio_lifestyle" val="notActive" color="secondary" label="Not Active" style="margin-left: 10px" />
                    </div>
                    <div class="col-xs-4 col-md-4">
                      <q-field label="Meds" />
                    </div>
                    <div class="col-xs-4 col-md-6">
                      <q-input type="text" id="meds-form" placeholder="Meds" />
                    </div>
                  </div>
                </div>
              </div>
            </q-card-main>
          </q-card>
          <!-- Custom Criteria Questions -->
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-title>Custom Criteria Question(s)
              <span slot="subtitle">Please enter additional criteria questions to filter the eligibility of the participants. The answers are yes/no.</span>
            </q-card-title>
            <q-card-main>
                 <div v-for="(criteriaQuestion, index) in criteriaQuestions" :key="criteriaQuestion.id">
                <q-btn class="vertical-top" v-show="index !==0" round size="sm" color="negative" icon="remove" @click="removeRowCriteriaQuestion(index)" />
                <q-field label="Criteria Question:" helper="Please enter a question. (e.g. Are you a smoker?)">
                  <q-input v-model="criteriaQuestion.titleOfCriteriaQuestion" type="text" clearable />
                </q-field>
                <q-field class="q-mt-md" label="Participant Answer:" helper="Please select either yes or no.">
                  <q-radio class="q-mr-lg" v-model="criteriaQuestion.criteriaQAnswer" val="yes" color="secondary" label="Yes" />
                  <q-radio v-model="criteriaQuestion.criteriaQAnswer" val="no" color="full" label="No" />
                </q-field>
                <q-btn class="float-right q-mt-sm" round size="sm" color="primary" icon="add" @click="addRowCriteriaQuestion(index)" />
                <q-card-separator class="q-mb-md q-mt-xl"/>
              </div>
            </q-card-main>
          </q-card>
        </q-tab-pane>
                <!-- TESTING Tab -->
        <q-tab-pane name="test">Tab Diseases
          <q-card class="bg-cyan-2  q-ma-xl">
          <!-- Disease -->
            <q-card-main>
              <q-chips-input v-model="diseasesVue" placeholder="Select disease(s) from list" @duplicate="duplicatedDisease">
                <q-autocomplete @search="searchDisease" @selected="selectedDisease" />
              </q-chips-input>
            </q-card-main>
            </q-card>
        </q-tab-pane>
        <!-- TESTING Tab 2 -->
         <!-- Meds -->
        <q-tab-pane name="test2">Tab Meds
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-main>
              <q-chips-input v-model="medsVue" placeholder="Select med(s) from list" @duplicate="duplicatedMeds">
                <q-autocomplete @search="searchMeds" @selected="selectedMeds" />
              </q-chips-input>
            </q-card-main>
          </q-card>
        </q-tab-pane>
      </q-tabs>
  </q-page>
</template>

<script>
import { required, between } from 'vuelidate/lib/validators'
import axios from 'axios'

export default {
  data () {
    return {
      studyTitle: '',
      studyDescription: '',
      dateStart: null,
      dateEnd: null,
      principalInvestigators: [
        {
          nameTitleOfInvestigator: '',
          contactDetailsOfInvestigator: '',
          institutionOfInvestigator: ''
        }
      ],
      institutions: [
        {
          nameOfInstitution: '',
          contactOfInstitution: '',
          radioDataAccessInstitution: ''
        }
      ],
      ageRangeMin: null,
      ageRangeMax: null,
      checkArrayGender: ['genderMale'],
      radio_lifestyle: 'Active',
      criteriaQuestions: [
        {
          titleOfCriteriaQuestion: '',
          criteriaQAnswer: ''
        }
      ],
      diseaseDescription: '',
      diseasesQueryResults: [],
      select: '',
      selectOptions: '',
      loading: false,
      medDescription: '',
      meds: {},
      diseases: {}
    }
  },
  validations: {
    studyTitle: { required },
    studyDescription: { required },
    dateStart: { required },
    ageRangeMin: { between: between(0, 140) },
    ageRangeMax: { between: between(0, 140) }
  },
  computed: {
    diseasesVue: {
      get: function () {
        console.log('GET')
        var keys = []
        for (let key in this.diseases) {
          keys.push(key)
        }
        console.log('GET', this.diseases)
        return keys
      },
      set: function (keys) {
        for (let key in this.diseases) {
          // if key is not in keys, delete
          if (!keys.includes(key)) delete this.diseases[key]
        }
        console.log('SET', this.diseases)
      }
    },
    medsVue: {
      get: function () {
        var keys = []
        for (let key in this.meds) {
          keys.push(key)
        }
        console.log('GET', this.meds)
        return keys
      },
      set: function (keys) {
        for (let key in this.meds) {
          // if key is not in keys, delete
          if (!keys.includes(key)) delete this.meds[key]
        }
        console.log('SET', this.meds)
      }
    }
  },
  methods: {
    searchMeds (medDescription, done) {
      // Declare top level URL vars
      var baseUrl = 'http://browser.ihtsdotools.org/api/v1/snomed/'
      var edition = 'en-edition'
      var version = '20180131'
      // Construct Meds Query URL
      var medQueryURL = baseUrl + '/' + edition + '/v' + version + '/descriptions?query=' + encodeURIComponent(medDescription) + '&limit=50&searchMode=partialMatching' + '&lang=english&statusFilter=activeOnly&skipTo=0' + '&semanticFilter=substance' + '&returnLimit=100&normalize=true'
      this.loading = true
      // axios.get('http://browser.ihtsdotools.org/api/v1/snomed//en-edition/v20180131/descriptions?query=heart%20attack&limit=50&searchMode=partialMatching&lang=english&statusFilter=activeOnly&skipTo=0&returnLimit=100&normalize=true')
      axios.get(medQueryURL)
        .then((response) => {
          this.loading = false
          this.$q.notify('Selected MEDS from S:' + JSON.stringify(Object.keys(this.meds)))
          const dataMed = response.data
          // needs to filter out those already selected
          let resMedsFiltByLen = dataMed.matches.filter(entry => entry['term'].length < 40)
          const selMeds = Object.keys(this.meds)
          console.log('RemMeds1: ', selMeds)
          var medsFil = resMedsFiltByLen.filter((entry) => !selMeds.includes(entry.term))
          console.log('disFil1: ', medsFil.length)
          if (medsFil.length === 0) {
            this.$q.notify('There are no more matching items with the current terms. Please search for other meds.')
          }
          const result = medsFil.map((item) => {
            return {
              label: item.term,
              value: item.term,
              conceptId: item.conceptId
            }
          })
          done(result)
          console.log(result)
        }, (error) => {
          console.log(error)
          this.loading = false
        })
    },
    selectedMeds (item) {
      this.$q.notify(`Selected suggestion "${item.label}"`)
      this.meds[item.label] = item.conceptId
      console.log('SELECTED', this.meds)
    },
    duplicatedMeds (label) {
      this.$q.notify(`"${label}" already in list`)
    },
    searchDisease (diseaseDescription, done) {
      // Declare top level URL vars
      var baseUrl = 'http://browser.ihtsdotools.org/api/v1/snomed/'
      var edition = 'en-edition'
      var version = '20180131'
      // Construct Disease Query URL
      var diseaseQueryURL = baseUrl + '/' + edition + '/v' + version + '/descriptions?query=' + encodeURIComponent(diseaseDescription) + '&limit=50&searchMode=partialMatching' + '&lang=english&statusFilter=activeOnly&skipTo=0' + '&semanticFilter=disorder' + '&returnLimit=100&normalize=true'
      this.loading = true
      // axios.get('http://browser.ihtsdotools.org/api/v1/snomed//en-edition/v20180131/descriptions?query=heart%20attack&limit=50&searchMode=partialMatching&lang=english&statusFilter=activeOnly&skipTo=0&returnLimit=100&normalize=true')
      axios.get(diseaseQueryURL)
        .then((response) => {
          this.loading = false
          this.$q.notify('Selected diseases from SEarch:' + JSON.stringify(Object.keys(this.diseases)))
          const dataDis = response.data
          // TODO: needs to filter out those already selected
          let resFiltByLen = dataDis.matches.filter(entry => entry['term'].length < 50)
          const selDis = Object.keys(this.diseases)
          console.log('RemDise1: ', selDis)
          var disFil = resFiltByLen.filter((entry) => !selDis.includes(entry.term))
          console.log('disFil1: ', disFil.length)
          if (disFil.length === 0) {
            this.$q.notify('There are no more matching items with the current terms. Please search for other diseases.')
          }
          const result = disFil.map((item) => {
            return {
              label: item.term,
              value: item.term,
              conceptId: item.conceptId
            }
          })
          done(result)
        }, (error) => {
          this.$q.notify('There has been an error during the retrieval of this query. Please Try again.')
          console.error(error)
          this.loading = false
        })
    },
    selectedDisease (item) {
      this.diseases[item.label] = item.conceptId
      console.log('SELECTED', this.diseases)
    },
    duplicatedDisease (label) {
      this.$q.notify(`"${label}" already in list`)
    },
    addRowInvestigator (index) {
      // increment the id
      this.principalInvestigators.push({
        nameTitle: '',
        contactDetails: '',
        institution: ''
      })
      this.$q.notify('the index is: ' + index)
    },
    removeRowInvestigator (index) {
      this.principalInvestigators.splice(index, 1)
    },
    addRowInstitution (index) {
      // increment the id
      this.institutions.push({
        nameOfInstitution: '',
        contactOfInstitution: ''
      })
    },
    removeRowInstitution (index) {
      this.institutions.splice(index, 1)
    },
    addRowCriteriaQuestion (index) {
      // increment the id
      this.criteriaQuestions.push({
        criteriaQuestion: ''
      })
    },
    removeRowCriteriaQuestion (index) {
      this.criteriaQuestions.splice(index, 1)
    },
    checkEndDate (dateStart, dateEnd) {
      if (Date.parse(dateStart) > Date.parse(dateEnd)) {
        this.$q.notify('The End Date of the study is before the Start Date. Please re-enter the End Date.')
        this.dateEnd = ''
      }
    },
    checkMaxAge (ageRangeMin, ageRangeMax) {
      if (ageRangeMax < ageRangeMin) {
        this.$q.notify('The maximum age of the participant is less than the minimum age. Please re-enter the maximum age.')
        this.ageRangeMax = ''
      }
    },
    submitButtonClicked () {
      this.$v.$touch()
      if (!this.$v.studyTitle.required) {
        this.$q.notify('Error not submitted')
      }
    }
  }
}
</script>

<style>
</style>
