<template>
    <!-- General Tab -->
        <q-tab-pane name="generalities">
          <div class="row">
            <div class="col-6"></div>
            <div class="col-6">
              <q-btn class="float-right q-mr-md" color="red" icon-right="save" @click="saveGeneralities">Save Progress</q-btn>
            </div>
          </div>
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
        </q-tab-pane>
</template>

<script>
import { required } from 'vuelidate/lib/validators'
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
      ]
    }
  },
  validations: {
    studyTitle: { required },
    studyDescription: { required },
    dateStart: { required }
  },
  methods: {
    saveGeneralities () {
      this.$q.notify('ave Gen: ' + this.studyTitle)
      this.$emit('genObj', this.studyTitle)
    },
    addRowInvestigator (index) {
      // increment the id
      this.principalInvestigators.push({
        nameTitleOfInvestigator: '',
        contactDetailsOfInvestigator: '',
        institutionOfInvestigator: ''
      })
    },
    removeRowInvestigator (index) {
      this.principalInvestigators.splice(index, 1)
    },
    addRowInstitution (index) {
      // increment the id
      this.institutions.push({
        nameOfInstitution: '',
        contactOfInstitution: '',
        radioDataAccessInstitution: ''
      })
    },
    removeRowInstitution (index) {
      this.institutions.splice(index, 1)
    },
    addRowCriteriaQuestion (index) {
      // increment the id
      this.criteriaQuestions.push({
        titleOfCriteriaQuestion: '',
        criteriaQAnswer: ''
      })
    },
    removeRowCriteriaQuestion (index) {
      this.criteriaQuestions.splice(index, 1)
    }
  }
}
</script>
